package testproject;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import java.time.Duration;

public class makemytrip {
    public static void main(String[] args) {
        // Initialize Chrome options
        ChromeOptions options = new ChromeOptions();
        options.addArguments("--start-maximized");

        // Initialize WebDriver
        WebDriver driver = new ChromeDriver(options);

        // Set implicit wait
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

        try {
            // Navigate to MakeMyTrip website
            driver.get("https://www.makemytrip.com");
            driver.findElement(By.xpath("//*[@id=\"SW\"]/div[1]/div[2]/div[2]/div/section/span")).click();
            

            // Wait for the page to load and click on the 'Buses' tab
            WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(20));
            WebElement busesTab = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//span[text()='Buses']")));
            busesTab.click();

            // Enter 'From' city
            WebElement fromCity = wait.until(ExpectedConditions.elementToBeClickable(By.id("fromCity")));
            fromCity.click();
            WebElement fromInput = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//input[@placeholder='From']")));
            fromInput.sendKeys("Chennai");
            WebElement fromOption = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//ul[@role='listbox']//li")));
            fromOption.click();

            // Enter 'To' city
            WebElement toCity = wait.until(ExpectedConditions.elementToBeClickable(By.id("toCity")));
            toCity.click();
            WebElement toInput = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//input[@placeholder='To']")));
            toInput.sendKeys("Bangalore");
            WebElement toOption = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//ul[@role='listbox']//li")));
            toOption.click();

            // Select travel date (e.g., 15th of the current month)
            WebElement datePicker = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//input[@id='travelDate']")));
            datePicker.click();
            WebElement date = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//div[@aria-label='Thu May 15 2025']")));
            date.click();

            // Click on the 'Search' button
            WebElement searchButton = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//button[text()='Search']")));
            searchButton.click();

            // Wait for search results to load
            wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//div[contains(@class, 'busCard')]")));

            // Print the title of the page
            System.out.println("Page title is: " + driver.getTitle());

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // Close the browser
            driver.quit();
        }
    }
}
