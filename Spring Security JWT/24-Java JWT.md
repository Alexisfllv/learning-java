## 07 CLASES PARA IMPLEMENTAR JWT 3.2


### JwtTokenUtil -  CLASE 01
``` java
@Component  
public class JwtTokenUtil implements Serializable {  
  
    private final long JWT_TOKEN_VALIDITY_SECONDS = 5 * 60 * 60 *1000;  
  
    @Value("${jwt.secret}")  
    private String secret;  
  
    public String generateToken(UserDetails userDetails) {  
  
  
        // agregar data al payload  
        Map<String, Object> claims = new HashMap<>();  
        claims.put("role", userDetails.getAuthorities()  
                .stream()  
                .map(e -> e.getAuthority())  
                .collect(Collectors.joining(",")));  
  
        claims.put("test", "value-test");  
        claims.put("test2", 50);  
  
        return doGenerateToken(claims, userDetails.getUsername());  
  
    }  
  
  
    private String doGenerateToken(Map<String, Object> claims, String username) {  
  
        SecretKey key = Keys.hmacShaKeyFor(secret.getBytes());  
  
        return Jwts.builder()  
                .claims(claims)  
                .subject(username)  
                .issuedAt(new Date(System.currentTimeMillis()))  
                .expiration(new Date(System.currentTimeMillis()+ JWT_TOKEN_VALIDITY_SECONDS))  
                .signWith(key)  
                .compact();  
    }  
  
    // validaciones  
    public Claims getallClaimsFromToken(String token){  
        SecretKey key = Keys.hmacShaKeyFor(secret.getBytes());  
  
        return Jwts.parser().verifyWith(key).build().parseClaimsJws(token).getPayload();  
  
    }  
  
    public <T> T getClaimsFromToken(String token, Function<Claims,T> claimsResolver){  
  
        final  Claims claims = getallClaimsFromToken(token);  
        return claimsResolver.apply(claims);  
    }  
  
    // saber el nombre de usuario del token  
    public String getUsernameFromToken(String token){  
        return getClaimsFromToken(token, e -> e.getSubject());  
    }  
  
    // saber el timepo de expiracion del token  
    public Date getExpirationDateFromToken(String token){  
        return getClaimsFromToken(token, e -> e.getExpiration());  
    }  
  
    //  
    private boolean isTokenExpired(String token){  
        final Date expiration = getExpirationDateFromToken(token);  
        return expiration.before(new Date());  
    }  
  
    public boolean validateToken(String token, UserDetails userDetails) {  
        final String username = getUsernameFromToken(token);  
        return (username.equals(userDetails.getUsername()) && !isTokenExpired(token));  
    }  
  
}
```

### JwtUserDetailsService Clase 02

``` java
@Service  
@RequiredArgsConstructor  
public class JwtUserDetailsService implements UserDetailsService {  
  
    private final UserRepo userRepo;  
  
    @Override  
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {  
        User user = userRepo.findOneByUserName(username);  
  
        if (user == null) {  
            throw new UsernameNotFoundException("User not found with username: " + username);  
        }  
  
        List<GrantedAuthority> roles = new ArrayList<>();  
        String rol =  user.getRole().getRoleName();  
        roles.add(new SimpleGrantedAuthority(rol));  
        return new org.springframework.security.core.userdetails.User(user.getUserName(), user.getPassword(),roles);  
  
    }  
}
```

###  JwtRequest Clase 03

```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
public class JwtRequest {  
  
    private String username;  
    private String password;  
  
}
```

### JwtResponse Clase 04

```java
public record JwtResponse(  
        String jwtToken  
) { }
```


### JwtRequestFilter Clase 05

``` java
@Component  
@RequiredArgsConstructor  
public class JwtRequestFilter extends OncePerRequestFilter {  
  
  
    private final JwtUserDetailsService jwtUserDetailsService;  
    private final JwtTokenUtil jwtTokenUtil;  
  
    @Override  
    protected void doFilterInternal(  
            HttpServletRequest request,  
            HttpServletResponse response,  
            FilterChain filterChain)  
            throws ServletException, IOException {  
  
        final String header =  request.getHeader("Authorization");  
  
        String username = null;  
        String jwtToken = null;  
  
  
        if (header != null && header.startsWith("Bearer ")) {  
            jwtToken = header.substring(7);  
  
            try {  
                username = jwtTokenUtil.getUsernameFromToken(jwtToken);  
            } catch (Exception e) {  
                request.setAttribute("error", e.getMessage());  
            }  
        }  
  
        if (username != null && jwtToken != null) {  
            UserDetails userDetails = jwtUserDetailsService.loadUserByUsername(username);  
  
            if (jwtTokenUtil.validateToken(jwtToken, userDetails)) {  
                UsernamePasswordAuthenticationToken auth =  
                        new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());  
  
                // sp3.0  
                auth.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));  
  
                SecurityContextHolder.getContext().setAuthentication(auth);  
            }  
        }  
  
        filterChain.doFilter(request, response);  
  
        // Bearer <Token>  
  
    }  
}
```

### JwtAuthenticationsEntryPoint Clase 06
```java
// Clase S6  
@Component  
public class JwtAuthenticationsEntryPoint implements AuthenticationEntryPoint {  
  
    @Override  
    public void commence(  
            HttpServletRequest request,  
            HttpServletResponse response,  
            AuthenticationException authException)  
            throws IOException, ServletException {  
  
        String message = (String)  request.getAttribute("error");  
  
        if (message == null) {  
            message = "Invalid token , failed autentication";  
        }  
  
        CustomErrorResponse errorResponse =  new CustomErrorResponse(LocalDateTime.now(),message, request.getRequestURI());  
  
        response.setStatus(HttpStatus.UNAUTHORIZED.value());  
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);  
  
        response.getWriter().write(converObjectecToJson(errorResponse));  
  
    }  
  
    // metodo de convertir a json  
    private String converObjectecToJson(Object object) throws JsonProcessingException {  
        if (object == null) {  
            return "null";  
        }  
        ObjectMapper mapper = new ObjectMapper();  
        mapper.findAndRegisterModules();  
        return mapper.writeValueAsString(object);  
    }  
}
```

### WebSecurityConfig - Clase 07

```java
@Configuration  
@EnableWebSecurity  
@EnableMethodSecurity  
@RequiredArgsConstructor  
public class WebSecurityConfig {  
  
    private final JwtAuthenticationsEntryPoint jwtAuthenticationsEntryPoint;  
    private final UserDetailsService userDetailsService;  
    private final JwtRequestFilter jwtRequestFilter;  
  
    @Bean  
    public AuthenticationManager authenticationManagerBean(AuthenticationConfiguration authenticationConfiguration) throws Exception {  
        return authenticationConfiguration.getAuthenticationManager();  
    }  
  
    @Bean  
    public static PasswordEncoder passwordEncoder() {  
        return new BCryptPasswordEncoder();  
    }  
  
    @Autowired  
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {  
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());  
    }  
  
    @Bean  
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {  
        // Desde spring boot 3.1+  
        http  
                .csrf(e -> e.disable())  
                        .authorizeHttpRequests(req -> req  
                                .requestMatchers(antMatcher("/login")).permitAll()  
                                .requestMatchers(antMatcher("/rest/**")).permitAll()  
                                .anyRequest().authenticated()  
                        )  
                        .exceptionHandling(e -> e  
                                .authenticationEntryPoint(jwtAuthenticationsEntryPoint)  
                        )  
                        .formLogin(e ->e.disable());  
        http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);  
  
        return http.build();  
    }  
}
```

### LoginController - Clase 08

```JAVA
@RestController  
@RequiredArgsConstructor  
public class LoginController {  
  
    private final AuthenticationManager authenticationManager;  
    private final JwtTokenUtil jwtTokenUtil;  
    private final UserDetailsService userDetailsService;  
  
    @PostMapping("/login")  
    public ResponseEntity<JwtResponse> login(@RequestBody JwtRequest jwtRequest) throws Exception {  
  
        authenticate(jwtRequest.getUsername(), jwtRequest.getPassword());  
        final UserDetails userDetails =  userDetailsService.loadUserByUsername(jwtRequest.getUsername());  
        final String token = jwtTokenUtil.generateToken(userDetails);  
  
        return ResponseEntity.ok(new JwtResponse(token));  
    }  
  
    private void authenticate(String username, String password) throws Exception {  
  
        try{  
            authenticationManager.authenticate(new UsernamePasswordAuthenticationToken(username, password));  
        }catch (DisabledException e){  
            throw new Exception("USER_DISABLED",e);  
        } catch (BadCredentialsException e){  
            throw new Exception("INVALID_CREDENTIALS",e);  
        }  
    }  
  
}
```