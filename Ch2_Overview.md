# Operating-Systems Services(作業系統服務)
## OS兩個觀點
- User view(使用者觀點): 好不好用？
- System view(系統觀點): 如何高效管理資源？
## OS的運作層次與服務
- User Interfaces: 提供與OS互動的方式。
  - GUI(圖形介面)
  - Touch screen(觸控介面)
  - Command line(命令列)
- System calls: 使用者程式與OS之間的橋樑，負責連接應用層與核心層。
- OS Services: 提供核心功能讓程式運作
  - Program execution(程式執行)
  - I/O operations(輸入輸出操作)
  - File systems(檔案系統)
  - Communication(資源分配)
  - Accounting(使用者帳務或使用紀錄)
  - Error detection(錯誤偵測)
  - Protection and security(保護與安全)

# User and Operating-System Interface(使用者與作業系統介面)
- 目的: OS提供介面，讓使用者能與系統互動、操作硬體與程式。
- 使用者介面種類:
  1. 命令解譯器(Command Interpreter, CLI)
    - 以文字指令操作系統。
    - 優點: 效率高、可自動化(script)。
    - 缺點: 需要記指令，學習曲線高。
  2. 圖形使用者介面(Graphical User Interface, GUI)
    - 以圖示、視窗、按鈕等圖形方式操作。
    - 優點: 直覺、容易使用。
    - 缺點: 需要較多資源(記憶體與運算)。
  3. 觸控螢幕介面(Touch Screen)
    - 使用手勢直接控制。
    - 結合GUI，提供更自然的互動方式。
# System Call(系統呼叫)
## OS是event-driven(事件驅動): 作業系統根據"事件"來反應與執行。
- Event可能是:
  1. Trap-Error
  2. Trap-system call
- Interrupt由硬體產生，用來通知OS有需要處理的事件。
- Trap由軟體產生，用來呼叫系統功能或處理錯誤。
- OS根據事件類型進入核心(Kernel)，執行對應的服務。
## What's System call?
- 定義:
  - 由OS提供給使用者程式呼叫的功能。
  - 是程式請求OS服務的主要介面。
  - 讓使用者空間程式(user space program)能請求核心層及服務
## System Call的種類
1. Process Control(行程控制): 對行程進行操作
   - 啟動、停止、刪除行程。
   - 分配記憶體空間
   - 讀取或設定行程屬性
   - 讓行程等待(suspend)
   - 範例: fork(), wait()
2. File Management(檔案管理): 對檔案進行操作
   - 建立、刪除檔案
   - 開啟、關閉檔案
   - 讀取、寫入資料
   - 取得或設定檔案屬性
   - 範例: open(), read(), write(), close()
3. Device Management(裝置管理): 管理硬體資源(device)
   - 向OS請求或釋放裝置
   - 取得或設定裝置屬性
   - 對裝置進行I/O操作
   - 範例: ioctl()
4. System Information Maintenance(系統資訊維護): 讀取與更新系統資料
   - 取得日期時間
   - 查詢統計資料
   - 存取process、file、device屬性
   - 範例: getpid(), getppid(), getuid()
5. Communication(行程間通訊): 不同行程間傳遞資料或同步
   - 建立/刪除通訊連線
   - 傳送與接收訊息
   - 存取通訊通道屬性
   - 範例: pipe(), socket(), accept(), connect()
6. Protection(保護): 管理權限與安全
   - 設定檔案或使用者權限
   - 控制系統資源存取
   - 範例: SetFileSecurity(), InitializeSecurity()
<img width="692" height="397" alt="image" src="https://github.com/user-attachments/assets/4be70b18-8cbb-41d5-a0be-d3ee80617e62" />


# Type of System call(系統呼叫的種類)
# System Services(系統服務)
# Linkers and Loaders(連結程式與載入程式)
# Why Applications are OS specific(為什麼應用程式依賴特定作業系統)
# Design and Implementation(設計與實作)
# OS Structure(作業系統結構)
# Building and Booting an OS(建置與開機過程)
# OS Debugging(作業系統除錯)
