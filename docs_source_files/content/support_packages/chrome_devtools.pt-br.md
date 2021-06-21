---
title: "Chrome Devtools"
weight: 10
---

As versões alfa do Selenium 4 têm aguardado suporte nativo para o protocolo Chrome DevTools por meio da interface "DevTools". Isso nos ajuda a obter propriedades de desenvolvimento do Chrome, como cache de aplicativo, busca, rede, desempenho, criador de perfil, tempo de recurso, segurança e domínios de CDP de destino, etc.

Chrome DevTools é um conjunto de ferramentas de desenvolvedor da web integradas diretamente no navegador Google Chrome. DevTools pode ajudá-lo a editar páginas dinamicamente e diagnosticar problemas rapidamente, o que, em última análise, ajuda a criar sites melhores e mais rápidos.

## Emulando Geolocalização:

Alguns aplicativos têm diferentes recursos e funcionalidades em diferentes locais. Automatizar esses aplicativos é difícil porque é difícil emular as localizações geográficas no navegador usando o Selenium. Mas com a ajuda de Devtools, podemos facilmente emulá-los. O trecho de código abaixo demonstra isso.

{{< code-tab >}}
  {{< code-panel language="java" >}}
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.devtools.DevTools;

public void geoLocationTest(){
  ChromeDriver driver = new ChromeDriver();
  Map coordinates = new HashMap()
  {{
      put("latitude", 50.2334);
      put("longitude", 0.2334);
      put("accuracy", 1);
  }};    
  driver.executeCdpCommand("Emulation.setGeolocationOverride", coordinates);
  driver.get("<your site url>");
}  
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
from selenium import webdriver
from selenium.webdriver.chrome.service import Service

def geoLocationTest():
driver = webdriver.Chrome()
Map_coordinates = dict({
"latitude": 41.8781,
"longitude": -87.6298,
"accuracy": 100
})
driver.execute_cdp_cmd("Emulation.setGeolocationOverride", Map_coordinates)
driver.get("<your site url>")
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
using System.Threading.Tasks;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium.DevTools;
// Replace the version to match the Chrome version
using OpenQA.Selenium.DevTools.V87.Emulation;

namespace dotnet_test {
  class Program {
    public static void Main(string[] args) {
      GeoLocation().GetAwaiter().GetResult();
    }

    public static async Task GeoLocation() {
      ChromeDriver driver = new ChromeDriver();
      DevToolsSession devToolsSession = driver.CreateDevToolsSession();
      var geoLocationOverrideCommandSettings = new SetGeolocationOverrideCommandSettings();

      geoLocationOverrideCommandSettings.Latitude = 51.507351;
      geoLocationOverrideCommandSettings.Longitude = -0.127758;
      geoLocationOverrideCommandSettings.Accuracy = 1;

      await devToolsSession
        .GetVersionSpecificDomains<OpenQA.Selenium.DevTools.V87.DevToolsSessionDomains>()
        .Emulation
        .SetGeolocationOverride(geoLocationOverrideCommandSettings);

        driver.Url = "<your site url>";
        }
    }
}
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
require 'selenium-webdriver'

driver = Selenium::WebDriver.for :chrome

begin
  # Latitude and longitude of Tokyo, Japan
  coordinates = { latitude: 35.689487,
                  longitude: 139.691706,
                  accuracy: 100 }
  driver.execute_cdp('Emulation.setGeolocationOverride', coordinates)
  driver.get 'https://www.google.com/search?q=selenium'
ensure
  driver.quit
end
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
// Please raise a PR to add code sample  
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
import org.openqa.selenium.chrome.ChromeDriver
import org.openqa.selenium.devtools.DevTools

fun main() {
    val driver =  ChromeDriver()
    val coordinates : HashMap<String, Any> = HashMap<String, Any> ()
    coordinates.put("latitude", 50.2334)
    coordinates.put("longitude", 0.2334)
    coordinates.put("accuracy", 1)
    driver.executeCdpCommand("Emulation.setGeolocationOverride", coordinates)
    driver.get("https://www.google.com")
}
  {{< / code-panel >}}
{{< / code-tab >}}

## Register Basic Auth:

Some applications require to keep some pages behind an auth and most of the time to keep things simple, a developer uses Basic Auth.
With Selenium and devtools integration, you can automate the input of basic auth credentials whenever they arise.

{{< code-tab >}}
  {{< code-panel language="java" >}}
Predicate<URI> uriPredicate = uri -> uri.getHost().contains("your-domain.com");

((HasAuthentication) driver).register(uriPredicate, UsernameAndPassword.of("admin", "password"));
driver.get("https://your-domain.com/login");
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const pageCdpConnection = await driver.createCDPConnection('page')

await driver.register('username', 'password', pageCdpConnection)
await driver.get(server.url())
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
{{< / code-tab >}}

## Listen to DOM events on a web page

Using Selenium's integration with CDP, one can listen to the DOM events and register callbacks to process the DOM event.

{{< code-tab >}}
  {{< code-panel language="java" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const cdpConnection = await driver.createCDPConnection('page')
await driver.logMutationEvents(cdpConnection, function(event) {
  assert.equal(event['attribute_name'], 'style')
  assert.equal(event['current_value'], '')
  assert.equal(event['old_value'], 'display:none;')
})

await driver.get(test.Pages.dynamicPage)

let element = driver.findElement({id: 'reveal'})
await element.click()
let revealed = driver.findElement({id: 'revealed'});
await driver.wait(until.elementIsVisible(revealed), 5000);
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
{{< / code-tab >}}

## Listen to JS Exceptions on a web page

Using Selenium's integration with CDP, one can listen to the JS Exceptions and register callbacks to process the exception details.

{{< code-tab >}}
  {{< code-panel language="java" >}}
import org.openqa.selenium.*;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.devtools.DevTools;

public void jsExceptionsExample() {
    ChromeDriver driver = new ChromeDriver();
    DevTools devTools = driver.getDevTools();
    devTools.createSession();

    List<JavascriptException> jsExceptionsList = new ArrayList<>();
    Consumer<JavascriptException> addEntry = jsExceptionsList::add;
    devTools.getDomains().events().addJavascriptExceptionListener(addEntry);

    driver.get("<your site url>");

    WebElement link2click = driver.findElement(By.linkText("<your link text>"));
    ((JavascriptExecutor) driver).executeScript("arguments[0].setAttribute(arguments[1], arguments[2]);",
        link2click, "onclick", "throw new Error('Hello, world!')");
    link2click.click();

    for (JavascriptException jsException : jsExceptionsList) {
        System.out.println("JS exception message: " + jsException.getMessage());
        System.out.println("JS exception system information: " + jsException.getSystemInformation());
        jsException.printStackTrace();
    }
}
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const cdpConnection = await driver.createCDPConnection('page')
await driver.onLogException(cdpConnection, function(event) {
  assert.equal(event['exceptionDetails']['stackTrace']['callFrames'][0]['functionName'], 'onmouseover')
})
await driver.get(test.Pages.javascriptPage)
let element = driver.findElement({id: 'throwing-mouseover'})
await element.click()
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
fun kotlinJsErrorListener() {
    val driver = ChromeDriver()
    val devTools = driver.devTools
    devTools.createSession()

    val logJsError = { j: JavascriptException -> print("Javascript error: '" + j.localizedMessage + "'.") }
    devTools.domains.events().addJavascriptExceptionListener(logJsError)

    driver.get("https://www.google.com")

    val link2click = driver.findElement(By.name("q"))
    (driver as JavascriptExecutor).executeScript(
        "arguments[0].setAttribute(arguments[1], arguments[2]);",
        link2click, "onclick", "throw new Error('Hello, world!')"
    )
    link2click.click()

    driver.quit()
}
  {{< / code-panel >}}
{{< / code-tab >}}

## Listen to console.log events on a web page

Using Selenium's integration with CDP, one can listen to the `console.log` events and register callbacks to process the event.

{{< code-tab >}}
  {{< code-panel language="java" >}}
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.devtools.DevTools;
import org.openqa.selenium.devtools.v87.log.Log;

public void consoleLogTest() {
ChromeDriver driver = new ChromeDriver();
DevTools devTools = driver.getDevTools();
devTools.createSession();

    devTools.send(Log.enable());
    devTools.addListener(Log.entryAdded(),
            logEntry -> {
                System.out.println("log: "+logEntry.getText());
                System.out.println("level: "+logEntry.getLevel());
            });
}
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
# Please raise a PR to add code sample
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const cdpConnection = await driver.createCDPConnection('page')
await driver.onLogEvent(cdpConnection, function(event) {
  assert.equal(event['args'][0]['value'], 'here')
})
await driver.executeScript('console.log("here")')
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
fun kotlinConsoleLogExample() {
    val driver = ChromeDriver()
    val devTools = driver.devTools
    devTools.createSession()

    val logConsole = { c: ConsoleEvent -> print("Console log message is: " + c.messages)}
    devTools.domains.events().addConsoleListener(logConsole)

    driver.get("https://www.google.com")

    val executor = driver as JavascriptExecutor
    executor.executeScript("console.log('Hello World')")

    val input = driver.findElement(By.name("q"))
    input.sendKeys("Selenium 4")
    input.sendKeys(Keys.RETURN)
    driver.quit()
}
  {{< / code-panel >}}
{{< / code-tab >}}

## Override Device Mode

Using Selenium's integration with CDP, one can override the current device mode and simulate a new mode. Width, height, mobile, and deviceScaleFactor are required parameters. Optional parameters include scale, screenWidth, screenHeight, positionX, positionY, dontSetVisible, screenOrientation, viewport, and displayFeature.

{{< code-tab >}}
{{< code-panel language="java" >}}
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.devtools.DevTools;

public void deviceSimulationTest() {
    ChromeDriver driver = (ChromeDriver) Driver.getDriver();
    tools = driver.getDevTools();
    tools.createSession();

    Map deviceMetrics = new HashMap()
    {{
        put("width", 600);
        put("height", 1000);
        put("mobile", true);
        put("deviceScaleFactor", 50);
    }};

    driver.executeCdpCommand("Emulation.setDeviceMetricsOverride", deviceMetrics);
    driver.get("https://www.google.com");
}
{{< / code-panel >}}
{{< code-panel language="python" >}}
# Please raise a PR to add code sample
{{< / code-panel >}}
{{< code-panel language="csharp" >}}
# Please raise a PR to add code sample
{{< / code-panel >}}
{{< code-panel language="ruby" >}}
# Please raise a PR to add code sample
{{< / code-panel >}}
{{< code-panel language="javascript" >}}
# Please raise a PR to add code sample
{{< / code-panel >}}
{{< code-panel language="kotlin" >}}
fun kotlinOverridDeviceMode() {
  val driver = ChromeDriver()

  val deviceMetrics: Map<String, Any> = object : HashMap<String, Any>() {
    init {
        put("width", 600)
        put("height", 1000)
        put("mobile", true)
        put("deviceScaleFactor", 50)
    }
  }

  driver.executeCdpCommand("Emulation.setDeviceMetricsOverride", deviceMetrics)
  driver.get("https://www.google.com")
  driver.quit()
}
{{< / code-panel >}}
{{< / code-tab >}}
