redo
====
package pulsewayclient;
 
import com.google.gson.Gson;
import java.io.IOException;
import java.util.logging.Level;
import java.util.logging.Logger;
import org.apache.commons.codec.binary.Base64;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.*;
 
public class PulsewayClient {
 
    private static final String ENDPOINT = "https://api.pulseway.com/v1/";
    // If you host your own Pulseway Enterprise Server, use:
    // private static final String ENDPOINT = "https://<server_name>/api/v1/"
    private static final String USERNAME = "username";
    private static final String APITOKEN = "24-13-12-C6-EC-A5-58-8E-"
                                         + "9C-2E-95-E9-37-84-02-44-"
                                         + "3C-4B-C3-E5-C7-FB-74-75-"
                                         + "D6-0D-6D-54-FC-7E-DB-2F-"
                                         + "30-18-60-E1-E3-92-03-0B-"
                                         + "52-66-91-46-7A-43-B8-31-"
                                         + "68-34-A9-45-97-AC-CE-C8-"
                                         + "93-47-99-49-EA-D7-2A-29";
    private static final String INSTANCE_ID = "production_website_1";
    private static final String INSTANCE_NAME = "Production Web Site";
    private static final String INSTANCE_GROUP = "Web Sites";
 
    public static void main(String[] args) {
        try {
            Gson gson = new Gson();
            HttpPost post = new HttpPost(ENDPOINT + "publish");
            NotifyRequest notifyRequest = getNotifyRequest();
            StringEntity notifyJson = new StringEntity(gson.toJson(notifyRequest));
            post.setEntity(notifyJson);
            post.setHeader("Content-Type", "application/json");
            HttpClient httpClient = HttpClients.createDefault();
            String auth = Base64.encodeBase64String((USERNAME + ":" + APITOKEN).getBytes());
            post.setHeader("Authorization", "Basic " + auth);
            HttpResponse response = httpClient.execute(post);
//            int code = response.getStatusLine().getStatusCode();
            System.out.println(response.toString());
        } catch (IOException ex) {
            Logger.getLogger(PulsewayClient.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
 
    private static NotifyRequest getNotifyRequest() {
        NotifyRequest request = new NotifyRequest();
        request.setInstance_id(INSTANCE_ID);
        request.setTitle("Could not connect to the database.");
        request.setMessage("Database Connection Error");
        request.setPriority("critical");
        return request;
    }
}
