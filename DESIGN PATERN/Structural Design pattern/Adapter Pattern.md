```java
import java.util.*;

interface PaymentGateway{
    void pay(String orderId, double amount);
}

class RazzorUPI{
    public void makePayments(String invioceID, double total){
        System.out.println("Payment done for" + " " + invioceID + " " + "for ₹" + total);
    }
}

class RazzorUPIGateway implements PaymentGateway{
    RazzorUPI razzorUPI;

    public RazzorUPIGateway(){
        this.razzorUPI = new RazzorUPI();
    }

    @Override
    public void pay(String orderID, double amount){
        razzorUPI.makePayments(orderID, amount);
    }
}

class PayUAPI{

    public void makePayments(String invioceID, double total){
        System.out.println("Payment done for" + " " + invioceID + " " + "for ₹" + total);
    }
}

class PayUAPIGateway implements PaymentGateway{
    PayUAPI payUAPI;

    public PayUAPIGateway() {
        this.payUAPI = new PayUAPI(); 
    }

    @Override
    public void pay(String orderId, double amount){
        payUAPI.makePayments(orderId, amount);
    }

    
}

class CheckoutService{
    PaymentGateway paymentGateway;

    public CheckoutService(PaymentGateway paymentGateway){
        this.paymentGateway = paymentGateway;
    }

    public void checkout(String orderId, double amount){
        paymentGateway.pay(orderId, amount);
    }
}

public class Main{
    @SuppressWarnings("ConvertToTryWithResources")
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        CheckoutService checkoutService = new CheckoutService(new PayUAPIGateway());
        checkoutService.paymentGateway.pay("123", 492.76);
        sc.close();
    }
}
```