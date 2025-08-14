``` mermaid
    flowchart

        subgraph Internet Zone
            user
        end

        subgraph Gateway Zone
            api-gateway
            auth-server
        end
        
        subgraph Services Zone
            product-service
            order-service
            payment-service
            delivery-service
            notification-service            
        end

        subgraph Infrastructure Zone           
            discovery-service
            config-server            
        end

        subgraph Observability Zone
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

        subgraph Database Zone
            mongo[(mongo)]
            postgress[(postgress)]
        end

        subgraph Kafka Zone
            orders-topic
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

        product-service --> order-service

        order-service --> payment-service

        payment-service --> delivery-service

        mailtrap-service --> user

```