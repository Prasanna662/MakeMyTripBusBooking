package testproject;

import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentTest;
import com.aventstack.extentreports.reporter.ExtentSparkReporter;

public class MakeMyTripBusBooking {

    WebDriver driver;
    WebDriverWait wait;
    ExtentReports extent;
    ExtentTest test;

    @BeforeClass
    public void setup() {
        // Setting up ChromeDriver
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10)); // Implicit wait

        // Setting up Extent Reports
        ExtentSparkReporter spark = new ExtentSparkReporter("BusBookingReport.html");
        extent = new ExtentReports();
        extent.attachReporter(spark);
        
     // Explicit wait setup
        wait = new WebDriverWait(driver, Duration.ofSeconds(20)); 
    }

    @Test(dataProvider = "busData")
    public void bookBusTicket(String fromCity, String toCity) throws InterruptedException {
        test = extent.createTest("Bus Booking Test: " + fromCity + " to " + toCity);
        driver.get("https://www.makemytrip.com/");

        try {
            // Close any initial popup or login overlay
            try {
                driver.findElement(By.cssSelector("body")).click();
            } catch (Exception e) {}

            // Click on Buses tab
            wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//span[text()='Buses']"))).click();
            
            // Enter FROM city
            WebElement fromInput = wait.until(ExpectedConditions.elementToBeClickable(By.id("fromCity")));
            fromInput.click();
            WebElement fromEdit = driver.findElement(By.xpath("//input[@placeholder='From']"));
            fromEdit.sendKeys(fromCity);
            wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//li//p[contains(text(),'" + fromCity + "')]"))).click();

            // Enter TO city
            WebElement toInput = wait.until(ExpectedConditions.elementToBeClickable(By.id("toCity")));
            toInput.click();
            WebElement toEdit = driver.findElement(By.xpath("//input[@placeholder='To']"));
            toEdit.sendKeys(toCity);
            wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//li//p[contains(text(),'" + toCity + "')]"))).click();

            // Select date
            wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//div[@aria-label][@role='gridcell']"))).click();

            // Click on Search button
            wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//button[text()='Search']"))).click();

            // Wait for search results to load
            wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//div[contains(text(),'Bus')]")));
            
            test.pass("Bus booking search completed successfully.");

        } catch (Exception e) {
            test.fail("Failed to complete bus booking test: " + e.getMessage());
        }
    }

    @DataProvider(name = "busData")
    public Object[][] getData() {
        return new Object[][] {
            {"Bangalore", "Chennai"},
            {"Hyderabad", "Pune"}
        };
    }

    @AfterClass
    public void tearDown() {
        driver.quit();
        extent.flush();
    }
} 
