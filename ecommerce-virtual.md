``` mermaid
    flowchart

        subgraph Internet Zone
            user
        end

        subgraph Gateway Zone
            api-gateway["api-gateway (port: 8080)"]
            auth-server["auth-server (port: 9090)"]
        end
        
        subgraph Services Zone
            product-service
            order-service
            payment-service
            delivery-service
            notification-service            
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
            mongo[(mongo)]
        end

        subgraph Database Sql Zone
            postgress[(postgress)]
        end

        subgraph Kafka Zone
            order-topic
            delivery-topic
            transactions-topic
        end

        user --> api-gateway

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

        api-gateway --> product-service
        product-service --> api-gateway

        api-gateway --> order-service
        order-service --> api-gateway

        product-service --> order-service
        order-service --> product-service

        order-service --> payment-service

        payment-service --> delivery-service

        mailtrap-service --> user

        order-service .-> order-topic .-> notification-service 

        delivery-service .-> delivery-topic .-> notification-service 

        order-service .-> transactions-topic
        transactions-topic .-> order-service

        payment-service .-> transactions-topic
        transactions-topic .-> payment-service

```