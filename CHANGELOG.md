# Changelog

All notable changes to DomoWatch are documented here.

Format: `[version] — date — summary`

---

## [1.0.1] — August 2026

### Added
- Over-The-Air (OTA) firmware updates via GitHub
  - Manual update check from the About popup
  - Download progress bar with visual feedback
  - Automatic rollback on boot failure (30-second validation window)
- French / English multilingual interface
  - All screens, web interface, and weather descriptions translated
  - Language switch with a single tap, saved across reboots
- Web configuration interface translated in active language

### Changed
- Custom partition layout for OTA dual-slot support (app0 3MB / app1 3MB / FFat ~10MB)
- WiFi link probe port changed from 53 to 80 — eliminates spurious reconnections on Freebox routers
- LVGL draw buffers moved to PSRAM — frees ~14KB of internal DRAM for TLS operations
- mbedTLS allocator redirected to PSRAM — resolves WiFiClientSecure failures under memory pressure
- Weather fetch deferred after WiFi connection to avoid xTaskCreate failures during DRAM peak
- NTP resync interval increased from 10s to 30s — prevents in-flight SNTP request cancellation
- Manual time setting applies immediately on arrow tap (removed Apply button)
- Config 3 screen now responds to horizontal swipe gesture
- Explanatory text colors lightened for better readability
- Clock date color lightened for improved visibility

### Fixed
- Race condition between wx_poll() and _wifi_poll() causing premature weather fetch
- geo_task and wx_task stack sizes reduced to 4096 bytes to prevent xTaskCreate failures
- Dead code removed: ui_clock_hide(), wx_label(), lcd_fill(), unused *_show() wrappers
- Color constants centralized in theme.h (eliminated hex value duplication)

---

## [1.0.0] — June 2026

### Initial release

- Digital and analog clock faces with NTP synchronization
- Automatic timezone from weather city (~300 IANA timezones)
- Live weather via Open-Meteo (temperature, condition, city search with geocoding)
- 18 configurable HTTP shortcut buttons (2 pages × 9)
- Optional confirmation popup per button
- Built-in web server for button configuration (no app required)
- JSON export/import for button configuration backup
- Adjustable backlight (50–100%) with real-time preview
- Robust WiFi: multi-AP, automatic reconnection, active link monitoring
- Physical button: backlight toggle (GPIO0)
- Tap on analog clock face: backlight toggle
