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
<img width="628" height="316" alt="image" src="https://github.com/user-attachments/assets/9f93bebe-ef9c-4a76-a525-dda8af264620" />


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
## Type of System Call
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

## System calls的運作流程
System call是user program與operating system kernel溝通的橋樑。透過trap讓cpu進入kernel mode，由OS執行請求的服務，再回到使用者程式。
## APIs
- 定義: 使用者程式呼叫system call的介面。是一個間接層，讓開發者能操作OS功能，而不需理解底層系統細節。
- 目的: 提供"標準化方式"讓開發者存取OS功能，不須知道底層system call實作。
- 常見三種API:
  - Windows API -> Windows
  - POSIX API -> Unix/Linux/macOS
  - Java API -> Java平台
## API與System call的關係
- API間接呼叫System call
- 例: 程式執行API，會透過C函式庫轉呼叫真正的system call，再由kernel處理。
## 為什麼不直接用system call，而用API？
1. 易於理解與使用: 不需知道system call細節。
2. 可攜性(Portability)高: 不同OS可共用相同API程式介面。
<img width="704" height="396" alt="image" src="https://github.com/user-attachments/assets/ede28c13-4637-4db9-8425-cc476291b3c7" />

## System call傳遞參數的三種方式
當使用者程式呼叫system call時，必須將參數傳給OS。不同架構與設計下，OS會用三種主要傳遞方式。
1. Registers
   - 原理: 在CPU的registers中直接存放參數。
   - 優點: 傳遞速度最快。
   - 缺點: registers數量有限，參數不可太多。
2. Memory + Registers
   - 原理: 參數先存在記憶體區塊(memory block)，再把該記憶體的"位址"放入registers傳給OS。
   - 優點: 可傳較多參數(不受registers數量限制)。
   - 缺點: 速度比純registers方式慢。
3. Stack
   - 原理: 利用程式堆疊依序壓入參數，系統呼叫時由OS從stack中取出。
   - 優點: 可傳任意多參數，擴充性最好。
   - 缺點: 速度最慢，需額外讀寫堆疊記憶體。
<img width="863" height="294" alt="image" src="https://github.com/user-attachments/assets/98d24b3f-a291-4c15-86e0-e93ce8cc13b0" />

# System Services(系統服務)
# Linkers and Loaders(連結程式與載入程式)

# Why Applications are OS specific(為什麼應用程式依賴特定作業系統)
- 因為不同OS的system call與API不同，應用程式需針對特定OS開發。

# Design and Implementation(設計與實作)
- 三大面向:
  1. Design Goal(設計目標):
     - user goal: 從user角度，系統要好用、容易上手、安全、反應快。
     - system goal: 從system角度，要好實作、好開發、好維護、高效率、有彈性。
  2. Mechanisms and Policies(機制與策略)
     - 要點: 機制與策略必須分開設計 -> 增加OS的彈性。
     - Mechanisms(機制): 提供動作的執行方法，如計時器。
     - Policy(策略): 決定動作的規則或準則。
  3. Implementation(實作):
     - 早期: 用組合語言撰寫；高效但難維護。
     - 現代: 使用高階語言C/C++；可攜性高、可維護性強、容易除錯。
<img width="791" height="216" alt="image" src="https://github.com/user-attachments/assets/9d5b0fac-44d2-4f15-834d-49ec235e8ae6" />

# OS Structure(作業系統結構)
1. 單一結構(Monolithic Structure):
   - 所有功能都集中在一個核心內(例如早期的MS-DOS、UNIX)。
   - 優點: 執行效率高。
   - 缺點: 系統龐大後維護困難；修改或新增功能容易導致錯誤。
2. 分層方法(Layered Approach):
   - OS被分成多層(Layer 0 ~ Layer N):
     - 最底層: 硬體
     - 最上層: user interface
   - 上層只能呼叫下層的功能。
   - 優點: 結構清楚、容易除錯；上層不須知道下層細節。
   - 缺點: 難以劃分層級間的功能邊界；執行效率下降(層層呼叫會變慢)。
3. 微核心(Microkernels):
   - 目標: 解決傳統OS龐大難維護的問題。
   - 將核心功能縮減為最基本部分(如排程、記憶體管理、通訊)。
   - 其他服務(如檔案系統、驅動程式)移到user mode執行。
   - 優點:
     1. 擴充容易: 新功能可直接加在user mode。
     2. 可移植性高: 核心小、修改少。
     3. 安全性高: 錯誤不會影響整個系統。
   - 缺點: performance(效能差)，user mode與kernel mode間頻繁傳遞訊息會降低速度。
4. 模組化(Modules):
   - 現代OS採用的主要架構。
   - 核心內部以模組形成組成，可動態載入或卸載。
   - 優點:
     1. 具備分層概念但更彈性，不受固定層及限制。
     2. 執行效率高，不必像微核心使用訊息傳遞。
5. 混和系統(Hybrid Systems):
   - 現代OS通常混和多種結構設計以兼顧效能與穩定性。
<img width="831" height="399" alt="image" src="https://github.com/user-attachments/assets/5a9c2cce-9728-4696-9299-87a728487e83" />

# Building and Booting an OS(建置與開機過程)
# OS Debugging(作業系統除錯)
