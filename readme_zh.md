```
# reliquary-archiver

_用于从某个回合制动漫游戏的网络包中创建遗物导出的工具_

json 输出格式基于 [HSR-Scanner](https://github.com/kel-z/HSR-Scanner) 的格式

旨在与 [fribbels hsr optimizer](https://github.com/fribbels/hsr-optimizer) 一起使用

## 运行

- 需要 [npcap](https://npcap.com/)（Windows）或 `libpcap`（Linux）
  - 在 Windows 上安装时，请确保启用了 "winpcap api-compatible mode"。
    如果此选项是灰色的，请参见[此处](https://github.com/IceDynamix/reliquary-archiver/issues/2)了解更多详细信息
    - 如果使用 WiFi，请启用 `Support raw 802.11 traffic (and monitor mode) for wireless adapters`
  - 在 Linux 上构建时，在生成的可执行文件上设置 `CAP_NET_RAW` 能力（通过 [pcap(3pcap)](https://man.archlinux.org/man/pcap.3pcap#Under~5)）
    ```sh
    sudo setcap CAP_NET_RAW=+ep target/release/reliquary-archiver
    ```
- 从[此处](https://github.com/IceDynamix/reliquary-archiver/releases/)下载最新版本
- **启动游戏并到达此屏幕。不要进入游戏**
![主菜单开始屏幕](./hsr_hyperdrive.jpg)
- 运行归档器可执行文件并等待其显示“listening with a timeout”
![归档器监听超时](./listening_for_timeout.png)
- 启动游戏
- 如果成功，归档器应输出一个文件 `archiver_output.json`
![归档器视觉指南](./archiver_visual_guide.gif)

可能需要禁用 VPN 或启用/禁用 WiFi！

### 命令行使用

```
Usage: reliquary-archiver.exe [OPTIONS] [OUTPUT]

Arguments:
  [OUTPUT]  输出 .json 文件的路径 [默认: archive_output.json]

Options:
      --pcap <PCAP>          从 .pcap 文件读取数据包而不是实时捕获数据包
      --timeout <TIMEOUT>    触发超时之前的等待时间（以秒为单位） [默认: 120]
  -v, --verbose...           输出的详细程度，可以设置最多 3 次。如果设置了 RUST_LOG，则无效
  -l, --log-path <LOG_PATH>  输出日志的路径
  -h, --help                 打印帮助信息
```

要自定义日志记录，可以
- 设置详细标志
- 或者设置 `RUST_LOG` 环境变量以自定义日志记录，参见 [此处](https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives)

要将日志输出到文件，请提供 `--log-path <路径>`。文件日志始终为 trace 级别。

## 从源代码构建

- 按照[此处](https://github.com/rust-pcap/pcap?tab=readme-ov-file#building)的说明进行
  - 对于我在 Windows 上，只需将 SDK 中的 `Packet.lib` 和 `wpcap.lib`（检查 x64 或 arm 目录）
    添加到此目录中即可成功链接
- `cargo build` / `cargo run`

请注意，必要的资源文件在构建脚本中下载并编译到二进制文件中。

## 相关项目

想要进行更多的数据包解析？查看归档器所基于的[独立库](https://github.com/IceDynamix/reliquary)！

想要导出你的成就？查看 [stardb-exporter](https://github.com/juliuskreutz/stardb-exporter)！

```
