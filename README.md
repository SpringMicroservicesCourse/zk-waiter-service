# Zookeeper 服務註冊與發現專案 ⚡

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.5-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-2024.0.2-blue.svg)](https://spring.io/projects/spring-cloud)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 專案介紹

這是一個基於 Spring Cloud 和 Apache Zookeeper 的微服務註冊與發現示範專案。主要展示如何在 Spring Boot 應用程式中整合 Zookeeper 作為服務註冊中心，實現服務的自動註冊、發現和負載均衡。

### 🎯 專案特色

- **服務自動註冊**：應用程式啟動時自動向 Zookeeper 註冊服務資訊
- **服務發現機制**：透過 Zookeeper 動態發現可用的服務實例
- **負載均衡**：支援多實例服務的負載分散
- **健康檢查**：整合 Spring Boot Actuator 提供服務健康狀態監控
- **監控指標**：使用 Micrometer 收集應用程式指標
- **快取機制**：整合 Spring Cache 提升效能

### 💡 為什麼選擇此專案？

- **學習價值**：深入理解微服務架構中的服務註冊與發現機制
- **實務應用**：可直接應用於企業級微服務專案
- **技術整合**：展示 Spring Boot、Spring Cloud、Zookeeper 的最佳整合方式
- **程式碼品質**：遵循最佳實踐，包含完整的註解和錯誤處理

## 技術棧

### 核心框架
- **Spring Boot 3.4.5** - 快速建構 Spring 應用程式的框架
- **Spring Cloud 2024.0.2** - 微服務架構的完整解決方案
- **Spring Data JPA** - 資料持久化層框架
- **Spring Cache** - 快取抽象層

### 服務註冊與發現
- **Apache Zookeeper 3.9.2** - 分散式協調服務，作為服務註冊中心
- **Spring Cloud Zookeeper Discovery** - Zookeeper 服務發現整合

### 開發工具與輔助
- **Lombok** - 減少樣板程式碼的註解處理器
- **Joda Money** - 貨幣處理函式庫
- **Apache Commons Lang3** - 通用工具類別庫
- **H2 Database** - 內嵌式關聯式資料庫
- **Micrometer** - 應用程式監控指標收集

## 專案結構

```
zk-waiter-service/
├── src/
│   ├── main/
│   │   ├── java/tw/fengqing/spring/springbucks/waiter/
│   │   │   ├── controller/           # REST API 控制器
│   │   │   │   ├── CoffeeController.java
│   │   │   │   ├── CoffeeOrderController.java
│   │   │   │   └── PerformanceInteceptor.java
│   │   │   ├── model/                # 資料模型
│   │   │   │   ├── BaseEntity.java
│   │   │   │   ├── Coffee.java
│   │   │   │   ├── CoffeeOrder.java
│   │   │   │   ├── OrderState.java
│   │   │   │   └── MoneyConverter.java
│   │   │   ├── repository/           # 資料存取層
│   │   │   │   ├── CoffeeRepository.java
│   │   │   │   └── CoffeeOrderRepository.java
│   │   │   ├── service/              # 業務邏輯層
│   │   │   │   ├── CoffeeService.java
│   │   │   │   └── CoffeeOrderService.java
│   │   │   ├── support/              # 支援類別
│   │   │   │   ├── CoffeeIndicator.java
│   │   │   │   ├── MoneyDeserializer.java
│   │   │   │   ├── MoneyFormatter.java
│   │   │   │   └── MoneySerializer.java
│   │   │   └── WaiterServiceApplication.java
│   │   └── resources/
│   │       ├── application.properties # 應用程式設定檔
│   │       ├── schema.sql            # 資料庫結構定義
│   │       ├── data.sql              # 初始資料
│   │       └── coffee.txt            # 咖啡資料檔案
│   └── test/
│       └── java/tw/fengqing/spring/springbucks/waiter/
│           └── WaiterServiceApplicationTests.java
├── pom.xml                           # Maven 專案設定檔
└── README.md
```

## 快速開始

### 前置需求
- **Java 21** 或更高版本
- **Maven 3.6+** 或更高版本
- **Docker** (用於執行 Zookeeper)
- **Git** (用於版本控制)

### 安裝與執行

1. **克隆此倉庫：**
```bash
git clone https://github.com/SpringMicroservicesCourse/spring-microservices-course.git
cd "Chapter 12 服務註冊與發現/zk-waiter-service"
```

2. **啟動 Zookeeper 服務：**
```bash
# 使用 Docker 啟動 Zookeeper
docker run -d --name zookeeper -p 2181:2181 zookeeper:3.9.2
```

3. **編譯專案：**
```bash
mvn clean compile
```

4. **執行應用程式：**
```bash
mvn spring-boot:run
```

5. **驗證服務註冊：**
```bash
# 連接到 Zookeeper 查看註冊的服務
docker exec -it spring-zookeeper zkCli.sh
# 在 Zookeeper CLI 中執行：
ls /services
get /services/waiter-service/[instance-id]
```

## 進階說明

### 環境變數
```properties
# Zookeeper 連線設定
spring.cloud.zookeeper.connect-string=localhost:2181

# 應用程式設定
spring.application.name=waiter-service
server.port=0  # 隨機端口

# 資料庫設定
spring.jpa.hibernate.ddl-auto=none
spring.jpa.show-sql=true

# 監控設定
management.endpoints.web.exposure.include=*
management.endpoint.health.show-details=always
```

### 設定檔說明
```properties
# application.properties 主要設定
spring.application.name=waiter-service                    # 服務名稱
spring.cloud.zookeeper.connect-string=localhost:2181     # Zookeeper 連線字串
server.port=0                                            # 隨機分配端口
spring.jpa.hibernate.ddl-auto=none                       # 不自動建立資料表
management.endpoints.web.exposure.include=*              # 開放所有監控端點
```

## API 端點

### 咖啡管理 API
- `GET /coffee/` - 取得所有咖啡清單
- `GET /coffee/{id}` - 根據 ID 取得咖啡資訊
- `GET /coffee/?name={name}` - 根據名稱取得咖啡資訊
- `POST /coffee/` - 新增咖啡 (支援 JSON 和表單格式)
- `POST /coffee/` (multipart) - 批次新增咖啡

### 訂單管理 API
- `GET /order/{id}` - 取得訂單資訊
- `POST /order/` - 建立新訂單

### 監控端點
- `GET /actuator/health` - 健康檢查
- `GET /actuator/info` - 應用程式資訊
- `GET /actuator/metrics` - 監控指標

## 參考資源

- [Spring Boot 官方文件](https://spring.io/projects/spring-boot)
- [Spring Cloud 官方文件](https://spring.io/projects/spring-cloud)
- [Apache Zookeeper 官方文件](https://zookeeper.apache.org/)
- [Spring Cloud Zookeeper Discovery 文件](https://docs.spring.io/spring-cloud-zookeeper/docs/current/reference/html/)

## 注意事項與最佳實踐

### ⚠️ 重要提醒

| 項目 | 說明 | 建議做法 |
|------|------|----------|
| Zookeeper 版本 | 生產環境建議使用穩定版本 | 使用 3.4.x 或 3.9.x 穩定版本 |
| 叢集部署 | 單點故障風險 | 最少部署 3 個節點，建議 5 個節點 |
| 網路延遲 | 跨機房部署問題 | 避免跨機房部署，使用本地叢集 |
| 服務降級 | 註冊中心故障處理 | 實作服務快取機制，確保服務可用性 |

### 🔒 最佳實踐指南

- **服務命名規範**：使用有意義的服務名稱，如 `waiter-service`、`customer-service`
- **健康檢查**：實作自定義健康檢查指標，監控服務狀態
- **錯誤處理**：妥善處理 Zookeeper 連線異常和服務發現失敗
- **監控指標**：使用 Micrometer 收集關鍵業務指標
- **快取策略**：對頻繁查詢的資料實作快取機制

### 📝 程式碼註解規範

在重要的程式碼區塊添加清楚註解，方便團隊成員理解與維護：

```java
/**
 * 咖啡服務類別
 * 負責處理咖啡相關的業務邏輯，包含新增、查詢、快取等功能
 * 
 * @author 開發團隊
 * @since 1.0.0
 */
@Service
@Slf4j
@CacheConfig(cacheNames = "CoffeeCache")
public class CoffeeService {
    
    /**
     * 儲存咖啡資訊
     * 將咖啡資料持久化到資料庫，並記錄操作日誌
     * 
     * @param name 咖啡名稱，不可為空
     * @param price 咖啡價格，使用 Joda Money 處理貨幣
     * @return 儲存後的咖啡實體
     */
    public Coffee saveCoffee(String name, Money price) {
        return coffeeRepository.save(Coffee.builder().name(name).price(price).build());
    }
}
```

## 授權說明

本專案採用 MIT 授權條款，詳見 LICENSE 檔案。

## 關於我們

我們主要專注在敏捷專案管理、物聯網（IoT）應用開發和領域驅動設計（DDD）。喜歡把先進技術和實務經驗結合，打造好用又靈活的軟體解決方案。

## 聯繫我們

- **FB 粉絲頁**：[風清雲談 | Facebook](https://www.facebook.com/profile.php?id=61576838896062)
- **LinkedIn**：[linkedin.com/in/chu-kuo-lung](https://www.linkedin.com/in/chu-kuo-lung)
- **YouTube 頻道**：[雲談風清 - YouTube](https://www.youtube.com/channel/UCXDqLTdCMiCJ1j8xGRfwEig)
- **風清雲談 部落格**：[風清雲談](https://blog.fengqing.tw/)
- **電子郵件**：[fengqing.tw@gmail.com](mailto:fengqing.tw@gmail.com)

---

**📅 最後更新：2025-08-11**  
**👨‍💻 維護者：風清雲談團隊**
