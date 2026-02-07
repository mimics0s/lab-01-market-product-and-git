## Product Choice
- **Product name** Wildberries.ru
- **Link** https://www.wildberries.ru
- **Short description** Online shop of fashionable clothes and other useful devices. 
## Main components
![Wildberries Component Diagram](../../../docs/diagrams/out/wildberries/architecture-component/Component%20Diagram.svg)
[Wildberries Component Diagram PlantUML](../../../docs/diagrams/src/wildberries/architecture-component.puml)
- **Cart & Checkout Service**
This service manages the user's shopping cart and handles the checkout process(calculating totals and connecting to the payment system).
- **Auth & ID Service**
It verifies user identity during login and manages user sessions.
- **Catalog & Search Service**
This component stores product information and helps users find products through searching.
- **Fintech & Payment Service**
It processes payments by connecting to banks and payment providers.
- **Notification Service**
This service sends alerts and updates to users via SMS, email, and mobile push notifications about orders and promotions.
## Data flow
![Wildberries Sequence Diagram](../../../docs/diagrams/out/wildberries/architecture-sequence/Sequence%20Diagram.svg)
[Wildberries Sequence Diagram PlantUML](../../../docs/diagrams/src/wildberries/architecture-sequence.puml)
**What happens:**
User adds item to cart -> system saves it in temporary storage -> updates cart total -> shows confirmation.
**Components and data:**
- User Alice -> Cart & Checkout Service:
product ID and quantity
- Cart & Checkout Service -> Redis:
saves cart with 7-day expiry
- Redis -> Cart & Checkout Service:
confirmation of save
- Cart & Checkout Service -> Client App:
new total price and badge counter
## Deployment
![Wildberries Deployment Diagram](../../../docs/diagrams/out/wildberries/architecture-deployment/Deployment%20Diagram.svg)
[Wildberries Deployment Diagram PlantUML](../../../docs/diagrams/src/wildberries/architecture-deployment.puml)
The components are deployed across different layers: users use **browsers** and **mobile apps**, which connect to **API gateways**. The **microservices** run in Kubernetes pods in the **compute cluster**, and data is stored in **database** and **storage clusters**.
## Assumptions
- I assume the **Notification Service** sends order updates using SMS and mobile push notifications from external companies.
- I assume the **Kafka Event Bus** helps different services talk to each other without waiting, especially after a payment is done.
## Open questions
- How exactly does the **Catalog & Search service** find products so fast when millions of users search at the same time?
- How does the **Payment Service** decide which bank or payment system to use for each customer's transaction?
