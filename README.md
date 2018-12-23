# istio-demo

## Presentation

  - [Istio_Presentation.pdf](./presentation/Istio_Presentation.pdf)

## Global Setup

  - Install Ingress Gateway for Default Namespace
  
    ```
    $ kubectl apply -f ./prereq/default-https-gateway.yaml
    ```
    
## [httpbin](https://istio.io/docs/tasks/traffic-management/ingress/)

- Install Workload

  ```
  $ kubectl apply -f ./httpbin/httpbin.yaml
  ```
  
- Create Virtual Service
 
   ```
   $ kubectl apply -f ./httpbin/httpbin-vs.yaml
   ```
 
- Demonstrate Endpoints

  - https://httpbin.k8s.example.com/ip
  - https://httpbin.k8s.example.com/headers
  - https://httpbin.k8s.example.com/delay/5

- Demo delay feature via web and CLI

  ```
  $ time curl -o /dev/null -s -w "%{http_code}\n" https://httpbin.k8s.example.com/delay/5
  ```
- Apply Timeout to Virtual Service

  ```
  $ kubectl apply -f ./httpbin/httpbin-3s-vs.yaml
  ```

- Demo Request Timeout

  ```
  $ time curl -o /dev/null -s -w "%{http_code}\n" https://httpbin.k8s.example.com/delay/1
  $ time curl -o /dev/null -s -w "%{http_code}\n" https://httpbin.k8s.example.com/delay/5
  ```
  
## [bookinfo](https://istio.io/docs/examples/bookinfo/)

The Bookinfo application is broken into four separate microservices:

productpage. The productpage microservice calls the details and reviews microservices to populate the page.
details. The details microservice contains book information.
reviews. The reviews microservice contains book reviews. It also calls the ratings microservice.
ratings. The ratings microservice contains book ranking information that accompanies a book review.
There are 3 versions of the reviews microservice:

Version v1 doesn’t call the ratings service.
Version v2 calls the ratings service, and displays each rating as 1 to 5 black stars.
Version v3 calls the ratings service, and displays each rating as 1 to 5 red stars.

- Install Workloads

  ```
  $ kubectl apply -f ./bookinfo/bookinfo.yaml
  $ kubectl apply -f ./bookinfo/networking/bookinfo-vs.yaml
  ```
  
  NOTE: Before you can use Istio to control the Bookinfo version routing, you need to define the available versions, called subsets, in destination rules.
  
- Define Destination Rules

  ```
  $ kubectl apply -f ./bookinfo/networking/destination-rule-all.yaml
  $ kubectl get destinationrules -o yaml
  ```
  
- Demo Application from Browser to demonstrate backend versions

  - Endpoints
    - https://bookinfo.k8s.example.com/productpage

- [Request Routing Tasks](https://istio.io/docs/tasks/traffic-management/request-routing/)
  - All traffic to one version
    
    ```
    $ kubectl apply -f ./bookinfo/networking/bookinfo-vs-all-v1.yaml
    ```
    
  - User Identity Based Routing
  
    ```
     $ kubectl apply -f ./bookinfo/networking/bookinfo-vs-user.yaml
    ```
   
  - Traffic Split (50/50)
   
    ```
    $ kubectl apply -f ./bookinfo/networking/bookinfo-vs-reviews-50-v3.yaml
    ```
    

