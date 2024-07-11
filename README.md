//Scenario 1: Updating My Profile
import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVRecord;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.Select;

import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;

public class UpdateProfile extends BaseTest {

    public void updateProfile() throws IOException {
        setUp();
        
        // Load user data from CSV
        Reader in = new FileReader("users.csv");
        Iterable<CSVRecord> records = CSVFormat.DEFAULT.withHeader().parse(in);

        for (CSVRecord record : records) {
            String username = record.get("username");
            String password = record.get("password");//Priyesh@0809
            String phone = record.get("phone");//+911234567890
            String address = record.get("address");//Kolkata
            String city = record.get("city");//Kolkata
            String postal_code = record.get("postal_code");//000000
            String is_seafarer = record.get("is_seafarer");
            String rank = record.get("rank");//

            // Login
            driver.get("https://onship.app/onship");
            driver.findElement(By.id("username")).sendKeys(username);//pritinanda0904@gmail.com
            driver.findElement(By.id("password")).sendKeys(password);//Priyesh@2020
            driver.findElement(By.id("login")).click();

            // Navigate to My Profile
            driver.findElement(By.linkText("My Profile")).click();

            // Update profile picture (assuming file input field exists)
            WebElement uploadElement = driver.findElement(By.id("profilePicture"));
            uploadElement.sendKeys("/path/to/profile/picture.jpg");

            // Update other fields
            driver.findElement(By.id("phone")).clear();
            driver.findElement(By.id("phone")).sendKeys(phone);//+911234567899
            driver.findElement(By.id("address")).clear();
            driver.findElement(By.id("address")).sendKeys(address);//Kolkata
            driver.findElement(By.id("city")).clear();
            driver.findElement(By.id("city")).sendKeys(city);//kolkata
            driver.findElement(By.id("postalCode")).clear();
            driver.findElement(By.id("postalCode")).sendKeys(postal_code);//000000

            // Update password
            driver.findElement(By.id("currentPassword")).sendKeys(password);//Priyesh@2020
            driver.findElement(By.id("newPassword")).sendKeys("newpassword");//0809@Priyesh
            driver.findElement(By.id("confirmNewPassword")).sendKeys("newpassword");//0809@Priyesh
            driver.findElement(By.id("updatePassword")).click();

            // Update position/role
            Select seafarerSelect = new Select(driver.findElement(By.id("isSeafarer")));
            seafarerSelect.selectByVisibleText(is_seafarer.equals("yes") ? "Yes" : "No");//yes

            if (is_seafarer.equals("yes")) {
                Select rankSelect = new Select(driver.findElement(By.id("rank")));
                rankSelect.selectByVisibleText(rank);
            } else {
                Select rankSelect = new Select(driver.findElement(By.id("rank")));
                rankSelect.selectByVisibleText(rank);
            }

            // Save changes
            driver.findElement(By.id("saveChanges")).click();

            // Logout
            driver.findElement(By.linkText("Logout")).click();
        }

        tearDown();
    }

    public static void main(String[] args) throws IOException {
        UpdateProfile updateProfile = new UpdateProfile();
        updateProfile.updateProfile();
    }
}

//Scenario 2: Verifying the Chat Messages Has Been Successfully Sent to Added Contacts

public class SendMessage extends BaseTest {

    public void sendMessage(String fromUser, String toUser, String message) {
        setUp();
        
        // Login as User-1
        driver.get("https://onship.app/onship");
        driver.findElement(By.id("username")).sendKeys(fromUser);//pritinanda0904@gmail.com
        driver.findElement(By.id("password")).sendKeys("password1");//0809@Priyesh
        driver.findElement(By.id("login")).click();

        // Navigate to Contacts
        driver.findElement(By.linkText("Contacts")).click();

        // Search and select User-2
        driver.findElement(By.id("searchContacts")).sendKeys(toUser);
        driver.findElement(By.linkText(toUser)).click();

        // Send message
        driver.findElement(By.id("messageInput")).sendKeys(message);
        driver.findElement(By.id("sendMessage")).click();

        // Logout
        driver.findElement(By.linkText("Logout")).click();

        tearDown();
    }

    public static void main(String[] args) {
        SendMessage sendMessage = new SendMessage();
        sendMessage.sendMessage("user1", "user2", "Hello, User-2!");
    }
}
//Scenario 3: Ensuring the User Can Receive and Respond to Chat Messages

public class ReceiveAndRespondMessage extends BaseTest {

    public void receiveAndRespondMessage(String fromUser, String toUser) {
        setUp();
        
        // Login as User-2
        driver.get("https://onship.app/onship");
        driver.findElement(By.id("username")).sendKeys(toUser);
        driver.findElement(By.id("password")).sendKeys("password2");
        driver.findElement(By.id("login")).click();

        // Navigate to Contacts
        driver.findElement(By.linkText("Contacts")).click();

        // Search and select User-1
        driver.findElement(By.id("searchContacts")).sendKeys(fromUser);
        driver.findElement(By.linkText(fromUser)).click();

        // Review message and respond
        WebElement receivedMessage = driver.findElement(By.className("receivedMessage"));
        System.out.println("Received message: " + receivedMessage.getText());

        driver.findElement(By.id("messageInput")).sendKeys("Hello, User-1!");
        driver.findElement(By.id("sendMessage")).click();

        // Logout
        driver.findElement(By.linkText("Logout")).click();

        tearDown();
    }

    public static void main(String[] args) {
        ReceiveAndRespondMessage receiveAndRespondMessage = new ReceiveAndRespondMessage();
        receiveAndRespondMessage.receiveAndRespondMessage("user1", "user2");
    }
}

