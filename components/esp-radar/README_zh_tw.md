# esp-radar 元件 [[English]](./README.md)

[![Component Registry](https://components.espressif.com/components/espressif/esp-radar/badge.svg)](https://components.espressif.com/components/espressif/esp-radar)

- [使用指南](https://github.com/espressif/esp-csi/tree/master/README.md)

Wi-Fi CSI（Channel State Information，通道狀態資訊）是藉由分析 Wi-Fi 訊號變化而取得的資訊。本專案提供 ESP-CSI 動作偵測演算法的示例。

### 將元件加入專案
請使用 component manager 的 `add-dependency` 指令，將 `esp-radar` 加入專案相依性；在 `CMake` 步驟中會自動下載該元件。

```
idf.py add-dependency "espressif/esp-radar=*"
```

## 範例
請使用 component manager 的 `create-project-from-example` 指令，從範本建立範例專案。

```
idf.py create-project-from-example "espressif/esp-radar=*:console_test"
```

執行後會在目前資料夾下載範例，您可以進入該範例進行建置與燒錄。

> 您也可以使用相同指令下載其他範例，或直接至 esp-radar 儲存庫取得：

 - [connect_rainmaker](https://github.com/espressif/esp-csi/tree/master/examples/esp-radar/connect_rainmaker)：在 ESP RainMaker 中加入 Wi-Fi CSI 功能
 - [console_test](https://github.com/espressif/esp-csi/tree/master/examples/esp-radar/console_test)：提供 Wi-Fi CSI 測試平台，涵蓋資料顯示、擷取與分析，協助快速理解 Wi-Fi CSI

### 常見問題
Q1. 使用套件管理器時遇到以下訊息：

```
  HINT: Please check manifest file of the following component(s): main

  ERROR: Because project depends on esp-radar (2.*) which doesn't match any
  versions, version solving failed.
```

A1. 透過指令下載的範例需註解每個範例 `main/idf_component.yml` 中的 `override_path` 行。

Q2. 使用套件管理器時出現以下錯誤：

```
Executing action: create-project-from-example
CMakeLists.txt not found in project directory /home/username
```

A2. 可能是套件管理器版本較舊，請在 ESP-IDF 環境中執行 `pip install -U idf-component-manager` 更新。
