EN ESTE PUNTO SE CREARON 5 CLASES PARA SOLUCIONAR EL PROBLEMA, ESTARE SEÑALIZANDO CUANDO EMPIEZA CADA UNA:
__________________________________________________________________________________________________________
import java.io.Serializable;
import java.util.Date;

public class Transaccion implements Serializable {
    private static final long serialVersionUID = 1L;
    private String id;
    private Date fechaHora;
    private String sucursalId;

    public Transaccion(String id, Date fechaHora, String sucursalId) {
        this.id = id;
        this.fechaHora = fechaHora;
        this.sucursalId = sucursalId;
    }


    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public Date getFechaHora() {
        return fechaHora;
    }

    public void setFechaHora(Date fechaHora) {
        this.fechaHora = fechaHora;
    }

    public String getSucursalId() {
        return sucursalId;
    }

    public void setSucursalId(String sucursalId) {
        this.sucursalId = sucursalId;
    }

    @Override
    public String toString() {
        return "Transaccion{" +
                "id='" + id + '\'' +
                ", fechaHora=" + fechaHora +
                ", sucursalId='" + sucursalId + '\'' +
                '}';
    }
}
_________________________________________________________________________________________________________
import java.io.Serializable;
import java.util.Date;

public class Compra extends Transaccion {
    private static final long serialVersionUID = 1L;
    private String productoId;
    private int cantidad;
    private double precioUnitario;

    public Compra(String id, Date fechaHora, String sucursalId, String productoId, int cantidad, double precioUnitario) {
        super(id, fechaHora, sucursalId);
        this.productoId = productoId;
        this.cantidad = cantidad;
        this.precioUnitario = precioUnitario;
    }


    public String getProductoId() {
        return productoId;
    }

    public void setProductoId(String productoId) {
        this.productoId = productoId;
    }

    public int getCantidad() {
        return cantidad;
    }

    public void setCantidad(int cantidad) {
        this.cantidad = cantidad;
    }

    public double getPrecioUnitario() {
        return precioUnitario;
    }

    public void setPrecioUnitario(double precioUnitario) {
        this.precioUnitario = precioUnitario;
    }

    @Override
    public String toString() {
        return "Compra{" +
                "productoId='" + productoId + '\'' +
                ", cantidad=" + cantidad +
                ", precioUnitario=" + precioUnitario +
                "} " + super.toString();
    }
}
__________________________________________________________________________________________________________________________
import java.io.Serializable;
import java.util.Date;

public class Venta extends Transaccion {
    private static final long serialVersionUID = 1L;
    private String productoId;
    private int cantidad;
    private double precioUnitario;

    public Venta(String id, Date fechaHora, String sucursalId, String productoId, int cantidad, double precioUnitario) {
        super(id, fechaHora, sucursalId);
        this.productoId = productoId;
        this.cantidad = cantidad;
        this.precioUnitario = precioUnitario;
    }


    public String getProductoId() {
        return productoId;
    }

    public void setProductoId(String productoId) {
        this.productoId = productoId;
    }

    public int getCantidad() {
        return cantidad;
    }

    public void setCantidad(int cantidad) {
        this.cantidad = cantidad;
    }

    public double getPrecioUnitario() {
        return precioUnitario;
    }

    public void setPrecioUnitario(double precioUnitario) {
        this.precioUnitario = precioUnitario;
    }

    @Override
    public String toString() {
        return "Venta{" +
                "productoId='" + productoId + '\'' +
                ", cantidad=" + cantidad +
                ", precioUnitario=" + precioUnitario +
                "} " + super.toString();
    }
}
______________________________________________________________________________________________________________________
import javax.jms.Connection;
import javax.jms.ConnectionFactory;
import javax.jms.Destination;
import javax.jms.JMSException;
import javax.jms.MessageProducer;
import javax.jms.Session;
import javax.jms.TextMessage;
import org.apache.activemq.artemis.jms.client.ActiveMQConnectionFactory;
import java.util.Date;

public class Sender {
    public static void main(String[] args) {
        ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://localhost:61616");
        try (Connection connection = connectionFactory.createConnection()) {
            connection.start();
            Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
            Destination destination = session.createQueue("transacciones");
            MessageProducer producer = session.createProducer(destination);

            Date fechaHora = new Date(); 
            Compra compra = new Compra("C1", fechaHora, "Sucursal1", "P001", 10, 150.0);
            TextMessage message = session.createTextMessage(compra.toString());
            producer.send(message);

            System.out.println("Mensaje enviado: " + compra);
        } catch (JMSException e) {
            e.printStackTrace();
        }
    }
}
_____________________________________________________________________________________________________________________
import javax.jms.Connection;
import javax.jms.ConnectionFactory;
import javax.jms.Destination;
import javax.jms.JMSException;
import javax.jms.MessageConsumer;
import javax.jms.MessageListener;
import javax.jms.Session;
import javax.jms.TextMessage;
import org.apache.activemq.artemis.jms.client.ActiveMQConnectionFactory;

public class Receiver {
    public static void main(String[] args) {
        ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://localhost:61616");
        try (Connection connection = connectionFactory.createConnection()) {
            connection.start();
            Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
            Destination destination = session.createQueue("transacciones");
            MessageConsumer consumer = session.createConsumer(destination);

            consumer.setMessageListener(new MessageListener() {
                @Override
                public void onMessage(javax.jms.Message message) {
                    if (message instanceof TextMessage) {
                        try {
                            String text = ((TextMessage) message).getText();
                            System.out.println("Mensaje recibido: " + text);
                        } catch (JMSException e) {
                            e.printStackTrace();
                        }
                    }
                }
            });

            Thread.sleep(10000);
        } catch (JMSException | InterruptedException e) {
            e.printStackTrace();
        }
    }
}
