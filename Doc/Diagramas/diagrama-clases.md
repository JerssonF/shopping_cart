# Diagrama UML de Clases (Implementación Actual)

Este diagrama refleja las clases principales que hoy existen en backend y gateway.

## Diagrama

```mermaid
classDiagram
        direction LR

        class Cart {
            +Long id
            +Long userId
            +CartStatus status
            +LocalDateTime createdAt
            +LocalDateTime updatedAt
            +touch()
        }

        class CartItem {
            +Long id
            +Long productId
            +String name
            +Integer quantity
            +BigDecimal price
            +BigDecimal subtotal
            +increaseQuantity(int)
            +updateQuantity(int)
            +updateProductSnapshot(String, BigDecimal)
            +recalculateSubtotal()
        }

        class ICartService {
            <<interface>>
            +createCart(Long)
            +addItemToCart(Long, AddCartItemRequestDTO)
            +updateCartItemQuantity(Long, Long, UpdateCartItemQuantityRequestDTO)
            +deleteCartItem(Long, Long)
            +getCartById(Long)
            +getCartTotal(Long)
        }

        class CartServiceImpl {
            -CartRepository cartRepository
            -CartItemRepository cartItemRepository
        }

        class CartRepository {
            <<repository>>
            +findFirstByUserIdAndStatus(Long, CartStatus)
        }

        class CartItemRepository {
            <<repository>>
            +findFirstByCartIdAndProductId(Long, Long)
            +findAllByCartIdOrderByIdAsc(Long)
        }

        class CartController {
            <<controller>>
            +createCart(...)
            +addItemToCart(...)
            +updateCartItemQuantity(...)
            +deleteCartItem(...)
            +getCartById(...)
            +getCartTotal(...)
        }

        class CartGatewayController {
            <<controller>>
        }

        class CartGatewayService {
            <<service>>
        }

        Cart "1" o-- "0..*" CartItem : items
        CartController --> ICartService
        ICartService <|.. CartServiceImpl
        CartServiceImpl --> CartRepository
        CartServiceImpl --> CartItemRepository
        CartGatewayController --> CartGatewayService
        CartGatewayService --> CartController : HTTP
```

## Notas

- El dominio principal está en `Cart` y `CartItem`.
- El gateway no implementa lógica de negocio de carrito, solo enruta hacia backend.
- Los DTOs fueron omitidos para mantener el diagrama legible, aunque sí están implementados.
