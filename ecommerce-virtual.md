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

        user --> api-gateway

        api-gateway --> auth-server
        auth-server --> api-gateway

        api-gateway --> product-service
        product-service --> api-gateway

        payment-service --> stripe-service
        stripe-service --> payment-service

        notification-service --> mailtrap-service
        mailtrap-service --> notification-service

```