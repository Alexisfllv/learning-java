## Consumiendo servicios Rest

```java
@RestController  
@RequestMapping("/rest")  
@RequiredArgsConstructor  
public class RestTemplateController {  
  
    private  final RestTemplate restTemplate;  
  
    @GetMapping("/pokemon/{name}")  
    public ResponseEntity<String> getPokemon(@PathVariable("name") String name){  
        String pokemonUrl= "https://pokeapi.co/api/v2/pokemon/" + name;  
        String response = restTemplate.getForEntity(pokemonUrl, String.class).getBody();  
  
        return ResponseEntity.ok(response);  
    }  
  
    @GetMapping("/test1")  
    public ResponseEntity<GenResponse<Category_DTO>> getTest1(){  
        String url = "http://localhost:8080/api/category";  
  
        HttpHeaders headers = new HttpHeaders();  
        headers.setAccept(List.of(MediaType.APPLICATION_JSON));  
  
        HttpEntity<String> entity = new HttpEntity<>(headers);  
  
        ParameterizedTypeReference<GenResponse<Category_DTO>> typeRef =  
                new ParameterizedTypeReference<>() {};  
        return restTemplate.exchange(url, HttpMethod.GET,entity,typeRef);  
    }  
  
    @GetMapping("/test2")  
    public ResponseEntity<String> getTest2(  
            @RequestParam(name = "page", defaultValue = "0") int page,  
            @RequestParam(name = "size", defaultValue = "5") int size  
    ){  
  
        String url = "http://localhost:8080/api/category/pagination2?page="+page+"&s="+size;  
        ResponseEntity<String> response = restTemplate.getForEntity(url, String.class);  
        return ResponseEntity.ok(response.getBody());  
    }  
  
    @GetMapping("/test3")  
    public ResponseEntity<Map> getTest3(  
            @RequestParam(name = "page", defaultValue = "0") int page,  
            @RequestParam(name = "size", defaultValue = "5") int size  
    ){  
  
        String url = "http://localhost:8080/api/category/pagination2?page={page}&s={size}";  
  
        Map<String,Integer> uriVariables = new HashMap<>();  
        uriVariables.put("page", page);  
        uriVariables.put("size", size);  
  
        HttpHeaders headers = new HttpHeaders();  
        HttpEntity<Object> requestEntity = new HttpEntity<>(headers);  
        return restTemplate.exchange(url,HttpMethod.GET,requestEntity,Map.class , uriVariables);  
  
    }  
  
    @PostMapping("/test4")  
    public ResponseEntity<Category_DTO> getTest4(@RequestBody Category_DTO category_DTO){  
        String url = "http://localhost:8080/api/category";  
        HttpEntity<Category_DTO> requestEntity = new HttpEntity<>(category_DTO);  
  
       Category_DTO response =  restTemplate.postForObject(url,requestEntity,Category_DTO.class);  
       return ResponseEntity.ok(response);  
    }  
  
    @PutMapping("/test5/{id}")  
    public ResponseEntity<Category_DTO> getTest5(@PathVariable ("id") int id, @RequestBody Category_DTO category_DTO){  
        String url = "http://localhost:8080/api/category/"+id;  
        HttpEntity<Category_DTO> requestEntity = new HttpEntity<>(category_DTO);  
  
        return restTemplate.exchange(url,HttpMethod.PUT,requestEntity,Category_DTO.class);  
  
    }  
  
  
    @DeleteMapping("test6/{id}")  
    public ResponseEntity<Void> getTest6(@PathVariable ("id") int id){  
        String url = "http://localhost:8080/api/category/"+id;  
        restTemplate.delete(url);  
        return ResponseEntity.noContent().build();  
    }  
  
    @DeleteMapping("test7/{id}")  
    public ResponseEntity<Void> getTest7(@PathVariable ("id") int id){  
        String url = "http://localhost:8080/api/category/{idCategory}";  
  
        Map<String,Integer> uriVariables = new HashMap<>();  
        uriVariables.put("idCategory",id);  
  
        restTemplate.delete(url,uriVariables);  
        return ResponseEntity.noContent().build();  
    }  
  
    @DeleteMapping("test8/{id}")  
    public ResponseEntity<Void> getTest8(@PathVariable ("id") int id, @RequestBody JwtRequest jwtRequest) throws Exception{  
        String token =  generateToke(jwtRequest.getUsername(), jwtRequest.getPassword());  
  
        // ENVIAR TOKEN PARA ACCIONES  
        HttpHeaders headers = new HttpHeaders();  
        headers.set("Content-Type",MediaType.APPLICATION_JSON_VALUE);  
        headers.set("Authorization","Bearer "+token);  
  
        HttpEntity<String> requestEntity = new HttpEntity<>(headers);  
  
        String url = "http://localhost:8080/api/category/{idCategory}";  
        Map<String,Integer> uriVariables = new HashMap<>();  
        uriVariables.put("idCategory",id);  
  
        restTemplate.exchange(url,HttpMethod.DELETE,requestEntity,String.class,uriVariables);  
        return ResponseEntity.noContent().build();  
  
    }  
  
    private String generateToke(String username, String password) throws Exception{  
        final String AUTENTICATION_URL = "http://localhost:8080/login";  
        JwtRequest jwtRequest = new JwtRequest(username, password);  
  
        String userRequestJson =  new ObjectMapper().writeValueAsString(jwtRequest);  
  
        HttpHeaders headers = new HttpHeaders();  
        headers.set("Content-Type", MediaType.APPLICATION_JSON_VALUE);  
        headers.set("Accept", MediaType.APPLICATION_JSON_VALUE);  
  
        HttpEntity<String> requestEntity = new HttpEntity<>(userRequestJson, headers);  
        ResponseEntity<JwtResponse> authResponse = restTemplate.exchange(AUTENTICATION_URL,HttpMethod.POST,requestEntity, JwtResponse.class);  
  
        if(authResponse.getBody() != null){  
            return authResponse.getBody().jwtToken();  
        }  
  
        return "INVALID_TOKEN";  
    }  
}
```