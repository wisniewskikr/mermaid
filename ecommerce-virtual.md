``` mermaid
    flowchart

        subgraph Internet Zone
            user
            admin
        end

        subgraph Gateway Zone
            api-gateway["api-gateway (port: 8080)"]
            auth-server["auth-server (port: 9090)"]
        end
        
        subgraph Services Zone
            product-service["product-service (port: 8081)"]
            order-service["order-service (port: 8082)"]
            payment-service["payment-service (port: 8083)"]
            delivery-service["delivery-service (port: 8084)"]
            notification-service["notification-service (port: 8085)"]            
        end

        subgraph Infrastructure Zone           
            discovery-service["discovery-service (port: 8761)"] 
            config-server["config-server (port: 8888)"]            
        end

        subgraph Observability Zone["Observability Zone (port: 3000)"]
            grafana-loki
            grafana-tempo
            grafana-prometheus  
        end

        subgraph Mailing Zone
            mailtrap-service
        end

        subgraph Payment Zone
            stripe-service
        end

        subgraph Database NoSql Zone
            mongo[(mongo port: 27017)]
        end

        subgraph Database Sql Zone
            postgress[(postgress port: 5432)]
        end

        subgraph Kafka Zone["Kafka Zone (port: 9092)"]
            order-topic
            delivery-topic
            transactions-topic
        end

        user --> api-gateway
        admin --> api-gateway

        api-gateway --> auth-server
        auth-server --> api-gateway        

        payment-service --> stripe-service
        stripe-service --> payment-service

        notification-service --> mailtrap-service
        mailtrap-service --> notification-service

        product-service --> mongo
        mongo --> product-service

        order-service --> postgress
        postgress --> order-service

        api-gateway -->|admin| product-service
        product-service -->|admin| api-gateway

        api-gateway -->|user| order-service
        order-service -->|user| api-gateway

        product-service -->|circuit breaker| order-service
        order-service -->|circuit breaker| product-service

        order-service -->|circuit breaker| payment-service

        payment-service -->|circuit breaker| delivery-service

        mailtrap-service --> user

        order-service .-> order-topic .-> notification-service 

        delivery-service .-> delivery-topic .-> notification-service 

        order-service .-> transactions-topic
        transactions-topic .-> order-service

        payment-service .-> transactions-topic
        transactions-topic .-> payment-service

        payment-service -->|circuit breaker| order-service
        delivery-service -->|circuit breaker| order-service

```