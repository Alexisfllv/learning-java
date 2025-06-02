

```java
@Entity
@Data = @Getter @Setter @EqualsAndHashCode @ToString
@AllArgsConstructor
@NoArgsConstructor
@Tabla
@EqualsAndHashCode(onlyExplicitlyIncluded = true)

@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@EqualsAndHashCode.Include
@Column(name = "", nullable = false, length 100)

PK
@Embeddable
@EqualsAndHashCode
@ManyToOne
@JoinColumn

@IdClass(IngressDetailPK.class)
```

---
# Relaciones de Java 
## Uno A muchos
## Sale
```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Entity  
@EqualsAndHashCode(onlyExplicitlyIncluded = true)  
public class Sale {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    @EqualsAndHashCode.Include  
    private Long idSale;  
  
  
    @Column( name = "sale_date_time",nullable = false)  
    private LocalDateTime saleDateTime;  
  
    @Column(name = "sale_total",nullable = false, precision = 10, scale = 2)  
    private BigDecimal saleTotal;  
  
    @Column( name = "sale_tax",nullable = false, precision = 10, scale = 2)  
    private BigDecimal saleTax;  
  
    @Column(name = "sale_enabled", nullable = false)  
    private boolean saleEnabled;  
  
    @ManyToOne  
    @JoinColumn(name = "id_client", nullable = false, foreignKey = @ForeignKey(name = "FK_SALE_CLIENT"))  
    private Client client;  
  
  
    @ManyToOne  
    @JoinColumn(name = "id_user", nullable = false, foreignKey = @ForeignKey(name = "FK_SALE_USER"))  
    private User user;  
  
  
    // una venta tiene muchos detalles  
    // todo lo que pase en cabezera pasa en detalle  
  
    @OneToMany(mappedBy = "sale", cascade = CascadeType.ALL)  
    private List<SaleDetail> details;  
}
```

## Detail Sale
```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Entity  
@EqualsAndHashCode(onlyExplicitlyIncluded = true)  
public class SaleDetail {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    @EqualsAndHashCode.Include  
    private Long idSaleDetail;  
  
    @Column(nullable = false)  
    private short quantity;  
  
    @Column(nullable = false, precision = 8, scale = 2)  
    private BigDecimal salePrice;  
  
    @Column(nullable = false, precision = 8, scale = 2)  
    private BigDecimal discount;  
  
    @ManyToOne  
    @JoinColumn(name = "id_sale", nullable = false, foreignKey = @ForeignKey(name = "FK_SALE_DETAIL_SALE"))  
    private Sale sale;  
  
    @ManyToOne  
    @JoinColumn(name = "id_product", nullable = false, foreignKey = @ForeignKey(name = "FK_SALE_DETAIL_PRODUCT"))  
    private Product product;  
}
```

## Muchos a Muchos  (Atributos)

## Product

``` java
@Entity  
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@EqualsAndHashCode(onlyExplicitlyIncluded = true)  
public class Product {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    @EqualsAndHashCode.Include  
    @Column(name = "id_product")  
    private Long idProduct;  
  
    @Column(name = "product_name",nullable = false, length = 50)  
    private String productName;  
  
    @Column(name = "product_price",nullable = false)  
    private BigDecimal productPrice;  
  
    @Column(name = "product_description",nullable = false, length = 250)  
    private String productDescription;  
  
    @Column(name = "product_enabled",nullable = false)  
    private Boolean productEnabled;  
  
    // fk  
  
    @ManyToOne  
    @JoinColumn(name = "id_category", nullable = false  
    , foreignKey = @ForeignKey(name = "FK_PRODUCT_CATEGORY"))  
    private Category category;  
  
}
```


## Ingress
``` java
@Entity  
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@EqualsAndHashCode(onlyExplicitlyIncluded = true)  
public class Ingress {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    @EqualsAndHashCode.Include  
    private Long idIngress;  
  
    @Column(name = "ingress_datetime", nullable = false)  
    private LocalDateTime ingressDateTime;  
  
    @Column(name = "ingress_total",nullable = false, precision = 10, scale = 2)  
    private BigDecimal ingressTotal;  
  
    @Column(name = "ingress_tax",nullable = false, precision = 10, scale = 2)  
    private BigDecimal ingressTax;  
  
    @Column(name = "ingress_serial_number", nullable = false)  
    private String ingressSerialNumber;  
  
    @Column(name = "ingress_enabled", nullable = false)  
    private boolean ingressEnabled;  
  
    // FKS  
  
    @ManyToOne  
    @JoinColumn(name = "id_provider", nullable = false, foreignKey = @ForeignKey(name = "FK_INGRESS_PROVIDER"))  
    private Provider provider;  
  
    @ManyToOne  
    @JoinColumn(name = "id_user", nullable = false, foreignKey = @ForeignKey(name = "FK_INGRESS_USER"))  
    private User user;  
  
  
}
```

## IngressDetail
```java
@Entity  
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@IdClass(IngressDetailPK.class)  
public class IngressDetail {  
  
    // llave primaria compuesta  
    @Id  
    private Ingress ingress;  
  
    @Id  
    private Product product;  
  
    @Column (nullable = false)  
    private short quantity;  
  
    @Column (nullable = false, precision = 8, scale = 2)  
    private BigDecimal cost;  
}
```

## IngressDetailPK
```java
@Embeddable  
@EqualsAndHashCode  
public class IngressDetailPK implements Serializable {  
  
  
    @ManyToOne  
    @JoinColumn(name = "id_ingress", nullable = false)  
    private Ingress ingress;  
  
    @ManyToOne  
    @JoinColumn(name = "id_product", nullable = false)  
    private Product product;  
}
```

## 