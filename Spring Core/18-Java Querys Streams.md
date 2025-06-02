# Mejor Vendedor
## Sql

```sql
select sum(s.sale_total) , ud.user_name 
from sale s 
inner join user_data ud on s.id_user = ud.id_user
group by ud.user_name;
```
## DTO

```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
public class MaxVendorUser_DTO {  
    String nameUser;  
    BigDecimal totalSales;  
}
```

## Service

``` java
// Obtener el mejor vendedor  
MaxVendorUser_DTO getBestSellerman();
```

## Impl

```java
@Override  
public MaxVendorUser_DTO getBestSellerman() {  
    return saleRepo.findAll()  
            .stream()  
            .collect(Collectors.groupingBy(  
                    sale -> sale.getUser().getUserName(), // usar el nombre del usuario  
                    Collectors.reducing(  
                            BigDecimal.ZERO,  
                            Sale::getSaleTotal,  
                            BigDecimal::add  
                    )  
            ))  
            .entrySet()  
            .stream()  
            .max(Map.Entry.comparingByValue()) // mayor total  
            .map(entry -> new MaxVendorUser_DTO(entry.getKey(), entry.getValue()))  
            .orElse(null);  
}
```

## Controller

```java
@GetMapping("/maxVendedorUser")  
public ResponseEntity<MaxVendorUser_DTO> maxVendedorUser(){  
    MaxVendorUser_DTO venta = saleService.getBestSellerman();  
    return ResponseEntity.status(HttpStatus.OK).body(venta);  
}
```

---
# Mejor Vendedor con mayor cantidad de Ventas
## Sql

```sql
select count(*) , ud.user_name
from sale s
inner join user_data ud on s.id_user = ud.id_user
group by ud.user_name;
```
## DTO

```java
public record MaxCountVendorUser_DTO(  
        String nameUser,  
        Integer countSales  
) {}
```

## Service

``` java
// Vendedor mayor cantidad de ventas
MaxCountVendorUser_DTO getBestSellermanWithMaxCount();
```

## Impl

```java
@Override  
public MaxCountVendorUser_DTO getBestSellermanWithMaxCount() {  
    return saleRepo.findAll()  
            .stream()  
            .collect(Collectors.groupingBy(  
                    sale -> sale.getUser().getUserName(),  
                    Collectors.collectingAndThen(  
                            Collectors.counting(),  
                            Long::intValue  
                    )  
            ))  
            .entrySet()  
            .stream()  
            .max(Map.Entry.comparingByValue())  
            .map(entry -> new MaxCountVendorUser_DTO(entry.getKey(), entry.getValue()))  
            .orElse(null);  
}
```

## Controller

```java
@GetMapping("/maxCountUser")  
public ResponseEntity<MaxCountVendorUser_DTO> maxCountUser(){  
    MaxCountVendorUser_DTO venta = saleService.getBestSellermanWithMaxCount();  
    return ResponseEntity.status(HttpStatus.OK).body(venta);  
}
```

---
# Mejor Vendedor , Cantidad y Ventas 
## Sql

```sql
SELECT 
    ud.user_name AS vendedor,
    COUNT(*) AS cantidad_ventas,
    SUM(s.sale_total) AS total_ventas
FROM 
    sale s
INNER JOIN 
    user_data ud ON s.id_user = ud.id_user
GROUP BY 
    ud.user_name;
```
## DTO

```java
public record SellerStatsDTO(  
        String nameUser,  
        BigDecimal totalSales,  
        Integer salesCount  
) {}
```

## Service

``` java
// mejor vendedor , total y cantidad  
SellerStatsDTO getSellerStats();
```

## Impl

```java
@Override  
public SellerStatsDTO getSellerStats() {  
    return saleRepo.findAll()  
            .stream()  
            // Agrupar por usuario y crear DTO con total y cantidad  
            .collect(Collectors.groupingBy(  
                    sale -> sale.getUser().getUserName(),  
                    Collectors.collectingAndThen(  
                            Collectors.toList(),  
                            list -> {  
                                BigDecimal totalSales = list.stream()  
                                        .map(Sale::getSaleTotal)  
                                        .reduce(BigDecimal.ZERO, BigDecimal::add);  
  
                                int countSales = list.size();  
  
                                return new SellerStatsDTO(list.get(0).getUser().getUserName(), totalSales, countSales);  
                            }  
                    )            ))  
            .values()  
            .stream()  
            // Obtener el que tiene el máximo totalSales  
            .max(Comparator.comparing(SellerStatsDTO::totalSales))  
            // Por si no hay ventas, devolver algo por defecto o null  
            .orElse(null);  
}
```

## Controller

```java
@GetMapping("/maxSeller")  
public ResponseEntity<SellerStatsDTO> maxSeller(){  
    SellerStatsDTO venta = saleService.getSellerStats();  
    return ResponseEntity.status(HttpStatus.OK).body(venta);  
}
```

---
# Mejor Producto Vendido
## Sql

```sql
select sum(sd.quantity) , p.product_name
from sale_detail sd 
inner join product p on sd.id_product = p.id_product
group by p.product_name;
```
## DTO

```java
public record ProductSalesDTO(  
        String productName,  
        Integer totalQuantity  
) {}
```

## Service

``` java
// Producto mayor vendido - sale detail  
List<ProductSalesDTO> getProductSalesSummary();
```

## Impl

```java
@Override  
public List<ProductSalesDTO> getProductSalesSummary() {  
    return saleDetailRepo.findAll()  
            .stream()  
            .collect(Collectors.groupingBy(  
                    sd -> sd.getProduct().getProductName(),  
                    Collectors.summingInt(SaleDetail::getQuantity)  
            ))  
            .entrySet()  
            .stream()  
            .map(entry -> new ProductSalesDTO(entry.getKey(), entry.getValue()))  
            .sorted(Comparator.comparing(ProductSalesDTO::totalQuantity).reversed()) // 👈 Orden descendente            .toList();  
}
```

## Controller

```java
// detail , cantidad mas vendida de producto  
@GetMapping("/detailMostProduct")  
public ResponseEntity<List<ProductSalesDTO>> detailMostProduct(){  
    List<ProductSalesDTO> venta = saleService.getProductSalesSummary();  
    return ResponseEntity.status(HttpStatus.OK).body(venta);  
}
```