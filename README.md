# Development of a SOAP Web service, WSDL with JAWRS

### The Web Service:
The W.S. contains:

1. **`Compte`** entity. This class is annotated with:
 -  **`@XmlRootElement`** is a Java annotation that indicates that a class should be treated as a root element in an XML document.
 -  **`@XmlAccessorType(XmlAccessType.FIELD) `** is a Java annotation that is used in conjunction with the JAXB (Java Architecture for XML Binding) API. It specifies that the fields of a class should be used as the source of data for the XML representation of the object instead of using the getters & setters as accessor methods.
 -  **`@XmlTransient`** is a Java annotation that indicates that a field or property should be excluded from the XML representation of an object.
 
2. **`BaunqueService`** class thta contains methods that allows:
 - converting an amount from Euro to MAD
 - consultation of an Account
 - consultation of a List of accounts
 This class is annotated is by **`@WebService`** annotation to mark it as a web service endpoint.
 Each of the methods in this class is marked with the annotation **`@WebMethod`** to mark it as a web service operation.
 
 3. **`ServeurJWS`** to host the web service.
 ```java 
 package ws;
import jakarta.xml.ws.Endpoint;
public class ServerJWS {
    public static void main(String[] args) {
        Endpoint.publish("http://0.0.0.0:8888/",new BanqueService());
        System.out.println("Web service déployé sur: http://0.0.0.0:8888/");
    }
}
 ```
The web service is deployed and hosted on the ServerJWS and can be accessed using the URL <http://0.0.0.0:8888/> 
 
---
### Consulting the WSDL:
The WSDL document describes the web service and defines the operations that can be performed by the service, as well as the messages that are exchanged between the service and its clients.

The WSDL document is typically used by a client application to understand how to interact with a web service. The client can use the information in the WSDL to generate code to send requests to the web service and process the responses.

To consult the WSDL document, we need to type in the URL <http://localhost:8888/?wsdl> whuch results in:

![image](https://user-images.githubusercontent.com/84817425/211078151-ac3f2aaa-9a13-4885-8588-5a66118f0b35.png)

### Testing the different W.S. functionnalities using SoapUI:

To do so:

1. we created a new Soap project and gave it the URL to the WSDL document.

![image](https://user-images.githubusercontent.com/84817425/211078758-f987166b-64d7-4260-849c-9785be4b531a.png)

This operation results in: 

![image](https://user-images.githubusercontent.com/84817425/211078940-a5a85262-1f64-4b64-a45b-85515cad0552.png)

2. we then tested the different OP. For example:

![Capture d’écran 2023-01-06 195102](https://user-images.githubusercontent.com/84817425/211079635-add01f95-58b9-4f9d-8334-1cabdc1a5ea1.png)

We sent a request containing the amount of `806 Euro` and the response contained the converted amount in `MAD`.

---
### SOAP client [Java]:
- After creating the java client, we installed the following plugin **`Jakarta EE: Web Services (JAX-WS)`**.
- we then generated a proxy from WSDL to communicate with the web service.
- Next we created a class that declares a proxy object (middleware) to retrieve the W.S. port.
Now, using this middleware we can call all the methods of the W.S. to use them.
```java
public class ClientWS {
    public static void main(String[] args) {
        //creation d'un middleware pour communiquer avec le webservice
        BanqueService stub=new BanqueWs().getBanqueServicePort();
        System.out.println(stub.convert(7600));
        Compte cp= stub.getCompte(5);
        System.out.println(cp.getCode());
        System.out.println(cp.getSolde());
    }
}
```
Running the main method results in :

![image](https://user-images.githubusercontent.com/84817425/211083045-eea4868a-1ace-4b40-9f57-1782e1fb487c.png)


