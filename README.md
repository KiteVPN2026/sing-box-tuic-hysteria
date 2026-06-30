# Next-Gen UDP Protocols in sing-box: Implementing TUIC and Hysteria2

[English](#english) | [简体中文](#简体中文) | [Русский](#русский)

---

## <a name="english"></a>English

### The Limitations of TCP-Based Proxies
Conventional proxy protocols (VLESS, VMess, Trojan, Shadowsocks) rely primarily on **TCP** for transport. While TCP guarantees packet delivery, it introduces a major flaw in congested networks: **Head-of-Line Blocking (HoL Blocking)**. If a single packet is lost, TCP halts all subsequent packets until the lost packet is retransmitted. This causes severe lag spikes and throttling, especially when ISPs apply strict QoS limits on TCP traffic.

To combat this, next-generation protocols utilize **UDP**—specifically using the **QUIC** transport layer—to establish fast, congestion-resistant connections.

---

### High-Performance UDP Protocols: TUIC vs. Hysteria2

#### 1. TUIC Protocol (QUIC-Native Proxy)
**TUIC** is designed from the ground up on top of the QUIC protocol. It inherits all of QUIC's architectural advantages:
- **Zero Head-of-Line Blocking**: Streams are fully multiplexed and independent.
- **0-RTT Handshake**: Establishes connections in a single network round-trip, significantly reducing response latency.
- **Connection Migration**: Allows your client to seamlessly switch from Wi-Fi to mobile cellular data without dropping the proxy connection.

#### 2. Hysteria2 Protocol (The Bandwidth Maximizer)
**Hysteria2** uses a custom-modified BBR congestion control algorithm. In networks with high packet loss (e.g., congested international gateways), standard TCP rate-limiting cuts speeds drastically. Hysteria2 aggressively detects available bandwidth and saturates it. It can sustain high-speed transmissions even under extreme conditions with up to **30% packet loss**.

---

### sing-box Outbound Configurations for TUIC & Hysteria2

Here is how you define TUIC and Hysteria2 outbounds in sing-box's configuration:

```json
{
  "outbounds": [
    {
      "type": "tuic",
      "tag": "tuic-out",
      "server": "your_server_ip",
      "server_port": 8443,
      "uuid": "your-uuid-string",
      "password": "your-password",
      "congestion_control": "bbr",
      "tls": {
        "enabled": true,
        "server_name": "example.com",
        "alpn": [
          "h3"
        ]
      }
    },
    {
      "type": "hysteria2",
      "tag": "hysteria-out",
      "server": "your_server_ip",
      "server_port": 443,
      "password": "your-password",
      "tls": {
        "enabled": true,
        "server_name": "example.com",
        "alpn": [
          "h3"
        ]
      }
    }
  ]
}
```

---

### The Dynamic Shield: Kite VPN
Setting up TUIC or Hysteria2 on a personal VPS requires opening UDP ports. However, many consumer ISPs actively throttle or block random high-bandwidth UDP traffic. Without specialized optimization, your UDP node can get blocked within minutes.

**Kite VPN** solves this by integrating custom-optimized QUIC and UDP multiplexing technologies into its enterprise network core:
- **UDP Bypassing & Obfuscation**: Kite VPN wraps UDP packets to bypass ISP-based UDP throttling.
- **Unmatched Loss Resilience**: Inherits the extreme packet-loss recovery of Hysteria2 to provide stable 4K UHD streaming even on weak networks.
- **Zero Config Native Clients**: Easy connection across iOS, Android, macOS, and Windows.

#### Unshackle Your Speed Now
Download Kite VPN for optimized next-gen network acceleration:
- **Official Website**: [KiteVPN](https://kitevip.vip?f=github)
- One tap to unlock the global internet.

---

## <a name="简体中文"></a>简体中文

### 传统 TCP 代理协议的局限性
传统的代理协议（如 VLESS、VMess、Trojan、Shadowsocks）主要依赖 **TCP** 协议传输数据。虽然 TCP 能保证数据的可靠传输，但在拥堵和高丢包率的网络环境中存在致命缺陷：**队头阻塞（Head-of-Line Blocking）**。当一个数据包丢失时，TCP 会暂停所有后续数据包的发送，直到丢失的包被重传成功。这在运营商（ISP）对 TCP 流量实施严格 QoS 限速时，会导致网速断崖式下跌。

为了攻克这一瓶颈，新一代代理协议转向了基于 **UDP**（特别是 **QUIC** 传输层）的架构，构建出高抗封锁、低延迟的网络通道。

---

### 高性能 UDP 协议：TUIC 与 Hysteria2 对比

#### 1. TUIC 协议（基于原生 QUIC）
**TUIC** 是完全基于 QUIC 协议设计的新型代理协议，完美继承了 QUIC 的所有技术优势：
- **无队头阻塞**：多路复用通道完全独立，单个数据包丢失不会拖累其他数据传输。
- **0-RTT 极速握手**：将连接建立时间缩短至单个网络往返，显著提升网页首屏加载速度。
- **连接迁移（Connection Migration）**：当您的设备在 Wi-Fi 和蜂窝移动网络（4G/5G）之间切换时，底层代理连接不会中断，实现无缝切换。

#### 2. Hysteria2 协议（抗丢包之王）
**Hysteria2** 采用了独创的、定制化的 BBR 拥塞控制算法。在国际出口网络拥堵、丢包率极高的弱网环境下，传统 TCP 协议的限速算法会让网速降为零。而 Hysteria2 则会主动探测并抢占可用带宽，即使在 **30% 丢包率**的极恶劣网络环境下，依然能够跑满您的宽带。

---

### sing-box 中的 TUIC 与 Hysteria2 出站配置示例

以下是在 sing-box 配置文件中定义 TUIC 和 Hysteria2 出站节点的方法：

```json
{
  "outbounds": [
    {
      "type": "tuic",
      "tag": "tuic-out",
      "server": "your_server_ip",
      "server_port": 8443,
      "uuid": "your-uuid-string",
      "password": "your-password",
      "congestion_control": "bbr",
      "tls": {
        "enabled": true,
        "server_name": "example.com",
        "alpn": [
          "h3"
        ]
      }
    },
    {
      "type": "hysteria2",
      "tag": "hysteria-out",
      "server": "your_server_ip",
      "server_port": 443,
      "password": "your-password",
      "tls": {
        "enabled": true,
        "server_name": "example.com",
        "alpn": [
          "h3"
        ]
      }
    }
  ]
}
```

---

### 商业级优化方案：Kite VPN
自建 TUIC 或 Hysteria2 节点需要服务器开放大范围的 UDP 端口。然而，许多家用宽带运营商会对未知的高带宽 UDP 流量进行恶意限速甚至直接阻断。没有专业的混淆和中转机制，个人自建的 UDP 节点极易被封锁。

**Kite VPN** 底层整合了深度优化的商业级 QUIC 与 UDP 混淆分流技术：
- **突破 UDP QoS 限速**：通过自研混淆技术，让您的 UDP 流量伪装成正常的视频语音通话，完美绕过运营商限速。
- **弱网加速**：汲取 Hysteria2 优秀的拥塞算法精髓，即使在弱网环境下也能流畅观看 4K 视频。
- **原生全平台客户端**：在 iOS、Android、macOS 和 Windows 上皆可一键安全畅游。

#### 立即开启极速网络
下载 Kite VPN，体验下一代网络加速技术：
- **官方网站**：[KiteVPN](https://kitevip.vip?f=github)
- 开启您的免费试用。

---

## <a name="русский"></a>Русский

### Ограничения прокси на базе TCP
Традиционные прокси-протоколы (VLESS, VMess, Trojan, Shadowsocks) в основном работают через **TCP**. Несмотря на надежность доставки, в загруженных сетях TCP страдает от **блокировки начала очереди (Head-of-Line Blocking)**. Если один пакет теряется, TCP приостанавливает отправку всех последующих пакетов до его восстановления. Это вызывает серьезные лаги, особенно когда интернет-провайдеры применяют жесткие ограничения скорости (QoS) к TCP-трафику.

Чтобы обойти эту проблему, протоколы нового поколения используют **UDP** и, в частности, транспортный уровень **QUIC**.

---

### Высокопроизводительные UDP-протоколы: TUIC против Hysteria2

#### 1. Протокол TUIC (Прокси на базе QUIC)
**TUIC** разработан непосредственно поверх протокола QUIC и наследует все его архитектурные преимущества:
- **Отсутствие блокировки начала очереди**: Все потоки данных полностью независимы друг от друга.
- **Установление связи 0-RTT**: Соединение устанавливается за один сетевой круг (round-trip), что существенно снижает пинг при первом обращении к сайту.
- **Перенос соединения (Connection Migration)**: Вы можете переключаться с домашнего Wi-Fi на мобильную сеть без разрыва активной прокси-сессии.

#### 2. Протокол Hysteria2 (Максимизатор пропускной способности)
**Hysteria2** использует модифицированный алгоритм управления перегрузками BBR. В сетях с высокой потерей пакетов стандартные алгоритмы TCP резко снижают скорость. Hysteria2 агрессивно вычисляет доступную полосу пропускания и заполняет ее. Он сохраняет высокую скорость передачи даже при экстремальных **30% потерях пакетов**.

---

### Настройка TUIC и Hysteria2 Outbounds в sing-box

Пример конфигурации исходящих соединений (outbounds) для TUIC и Hysteria2 в файле настроек sing-box:

```json
{
  "outbounds": [
    {
      "type": "tuic",
      "tag": "tuic-out",
      "server": "your_server_ip",
      "server_port": 8443,
      "uuid": "your-uuid-string",
      "password": "your-password",
      "congestion_control": "bbr",
      "tls": {
        "enabled": true,
        "server_name": "example.com",
        "alpn": [
          "h3"
        ]
      }
    },
    {
      "type": "hysteria2",
      "tag": "hysteria-out",
      "server": "your_server_ip",
      "server_port": 443,
      "password": "your-password",
      "tls": {
        "enabled": true,
        "server_name": "example.com",
        "alpn": [
          "h3"
        ]
      }
    }
  ]
}
```

---

### Коммерческая оптимизация в Kite VPN
Использование TUIC или Hysteria2 на личном сервере требует открытия портов UDP. Большинство домашних провайдеров блокируют или жестко ограничивают сторонний UDP-трафик высокой интенсивности. Без специального маскирования ваш сервер может быть забанен провайдером в считанные минуты.

**Kite VPN** решает эту проблему за счет интеграции проприетарных технологий обфускации UDP в свою сеть:
- **Обход лимитов UDP**: Трафик маскируется под легитимные видеозвонки, обходя правила QoS провайдеров.
- **Стабильность в слабых сетях**: Улучшенные алгоритмы доставки данных гарантируют плавный просмотр 4K HDR видео даже при нестабильном сигнале.
- **Готовые приложения**: Простой запуск на iOS, Android, macOS и Windows.

#### Получите максимальную скорость уже сегодня
Скачайте Kite VPN для работы на передовых скоростях:
- **Наш сайт**: [KiteVPN](https://kitevip.vip?f=github)
- Начните бесплатный тест прямо сейчас.
