This is the c++ script to run inside arduino IDE to make your own AP(acess point) scanner.

#include <WiFi.h>

void setup() {
  Serial.begin(115200);
  delay(1000);

  WiFi.mode(WIFI_STA);  // Enable Wi-Fi scanning
  WiFi.disconnect();
  delay(100);

  Serial.println("Wi-Fi access point scanner");
}

void loop() {
  Serial.println("\nScanning...");
  int count = WiFi.scanNetworks();

  if (count < 0) {
    Serial.printf("Scan failed: %d\n", count);
  } else if (count == 0) {
    Serial.println("No access points found.");
  } else {
    Serial.printf("Found %d access point(s):\n", count);

    for (int i = 0; i < count; i++) {
      Serial.printf(
        "%2d. SSID: %-32s RSSI: %4d dBm  Channel: %2d  Security: %s\n",
        i + 1,
        WiFi.SSID(i).c_str(),
        WiFi.RSSI(i),
        WiFi.channel(i),
        WiFi.encryptionType(i) == WIFI_AUTH_OPEN ? "Open" : "Secured"
      );
    }
  }

  WiFi.scanDelete();  // Release scan-result memory
  delay(10000);       // Scan again in 10 seconds
}
