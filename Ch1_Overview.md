# 作業系統的功能:
作業系統是使用者與硬體之間的介面，負責分配與管理資源、解決衝突並監控程式執行，以提供方便、安全且有效率的運作環境。

# 電腦系統的組成:
電腦系統是由 CPU、記憶體與多個裝置控制器所構成，它們透過系統匯流排(System Bus)互相連接並共享記憶體。
- 一個系統可能包含一個或多個CPU。
- 每個控制器負責管理特定裝置，且都有對應的驅動程式(Driver)。
- 不同裝置可同時運作，但會互相競爭記憶體的存取權。

## 裝置控制器(Device Controller):
- 屬於硬體。
- 負責控制特定的I/O裝置。
- 內含：
  - 特殊用途暫存器(special-purpose registers)：存放控制與狀態資訊。
  - 本地緩衝區(local buffer)：暫存資料以便與記憶體交換。
- ### CPU不直接控制裝置，而是透過控制器進行資料傳輸與命令執行。

## 裝置驅動程式(Device Driver):
- 屬於軟體，為作業系統的一部份。
- 每個控制器都有對應的驅動程式。
- 驅動程式負責將OS的高階命令轉換成控制器能理解的低階指令。

## 電腦系統運作的三個關鍵:
- 中斷(Interrupt): 讓CPU在執行指令時能被外部事件打斷並即時處理。
- 儲存體結構(Storage Structure): 確保資料能在CPU、記憶體與外部裝置間正確傳遞。
- I/O結構(Input/Output Structure): 管理裝置之間的資料交換與控制訊號。

### 1. 中斷(Interrupt):
**定義** 
- 中斷是硬體或軟體事件發生時，發送給CPU的訊號，用來要求暫停目前工作並優先處理該事件。

**目的**
- 讓CPU不必一直等待裝置完成任務，提高效率；使系統能即時回應外部事件。

**運作流程**
  1. Device Controller偵測到事件，發出IRQ(中斷請求)。
  2. CPU停止目前指令的執行，保存現有狀態。
  3. CPU透過中斷向量表(IVT)找出對應的中斷服務程式。
  4. OS中的IRS處理該事件(如讀取資料、重設狀態)。
  5. IRS結束，CPU恢復原先執行狀態並繼續原程式。

**系統元件**
  - IRQ(Interrupt Request Line): 硬體線路，用來傳送中斷請求。
  - PIC(Programmable Interrupt Controller): 管理多個IRQ的輸入，負責排序、篩選並傳送中斷給CPU。
  - IVT(Interrupt Vector Table): 記錄各種中斷對應的IRS位址，供CPU查找。
  - ISR(Interrupt Service Routine): 實際的中斷處理程式，由OS執行。
  
**重點概念**
  - 硬體中斷: 由外部裝置(如鍵盤、磁碟)產生。
  - 軟體中斷: 由程式主動觸發，用於系統呼叫或例外處理。
  - 優點: 提升系統反應速度與資源利用率。
### 小總結: 中斷是讓CPU能即時回應外部事件、保持系統高效運作的核心機制。

### 2. 儲存體結構(Storage Structure):
- CPU只能從記憶體載入程式執行。
- 所有被執行的程式與資料都必須先存在記憶體中。
- 儲存體依速度與容量分層，用來平衡"效能、容量、成本"。
  - 儲存體分類:
    1. 揮發性(Volatile): 斷電即失，如RAM。
    2. 非揮發性(Non-Volatile): 即使斷電仍保存資料，如SSD、HDD。
  - 主要儲存體(Primary):
    1. Register(暫存器): CPU內部最快、容量最小的儲存單元。
    2. Cache(快取記憶體): 暫存常用資料，加速CPU存取主記憶體的速度。
    3. RAM(主記憶體): CPU可直接存取，用於執行程式與暫存資料。
  - 輔助儲存體(Secondary):
    1. 主要的永久資料儲存裝置，如SSD、HDD。
    2. 電腦必須依靠這些儲存體才能開機或執行作業系統。
  - 三級儲存體(Tertiary):
    1. 使用可移除式媒體，如光碟、磁帶。
    2. 常用於資料備份或大量長期存放。
  - 速度與容量:
    - 由上至下(暫存>>快取>>記憶體>>硬碟>>光碟): 速度變慢、容量變大、價格變低。
    - 系統需在"處理速度"與"儲存容量"間取得平衡。
### 小總結: 儲存體結構是由多層次記憶體組成的階層系統，用以兼顧效能、容量與成本，使CPU能高效執行程式並穩定儲存資料。

### 3. I/O結構(Input/Output Structure):
- Polling I/O:
  - 運作方式:
    - CPU不斷主動檢查IO裝置是否完成工作。
    - 若未完成，CPU持續等待(busy waiting)。
  - 特點:
    - 結構簡單，早期系統常用。
    - CPU無法同時執行其他任務。
    - 效率最低。
- Interrupt I/O:
  - 運作方式:
    - CPU繼續執行其他程式。
    - 當IO完成時，Device Controller發送中斷信號。
    - CPU暫停當前工作，執行ISR處理IO，再回到原工作。
  - 特點:
    - CPU不需一直等待，可更有效率利用時間。
    - 適合鍵盤、磁碟、網路等裝置。
    - 現代作業系統常用。
- DMA(Direct Memory Access):
  - 運作方式:
    - 由DMA控制器直接負責記憶體與IO裝置間的資料傳輸。
    - CPU只需下命令，不參與搬運過程。
    - 傳輸完成後，DMA發送中斷通知CPU。
  - 特點:
    - CPU幾乎完全解放，效率最高。
    - 適合大量資料傳輸，如磁碟、影像、網路。
    - 較複雜但效能最佳。
- ### 小總結: Polling是由CPU親自檢查，Interrupt是由裝置通知CPU，DMA是由裝置自己處理並告訴CPU"已完成"。
## 本總結: 電腦系統由CPU、記憶體與多個裝置控制器組成，透過系統匯流排(System Bus)互連並共享記憶體。CPU負責執行指令，記憶體提供資料暫存與載入環境，而各控制器則管理其專屬的I/O裝置，並透過驅動程式(Device Driver)與作業系統溝通。整個系統運作依賴三大核心機制：中斷(Interrupt)、儲存體結構(Storage Structure) 與I/O結構(Input/Output Structure)。中斷讓CPU能即時回應外部事件並維持高效率；儲存體以多層次階層設計（暫存器、快取、主記憶體、硬碟等）在效能與容量間取得平衡；而I/O結構則負責資料交換，從最基本的Polling到高效的DMA直接記憶體存取，逐步減少CPU參與、提升系統整體效能。這三者協同運作，使電腦能穩定、即時且高效地完成各類任務。

# 電腦系統的架構:
- Single-Processor System(單一處理器系統)
- Multiprocessor System(多處理器系統)
- Clustered System(叢集系統)
## Single-Processor System:
- 只有一個CPU核心
- 該核心包含Registers與Cache，用於指令執行與資料暫存。
- 所有作業都由這顆CPU處理，因此同一時間只能執行一組指令。
## Multiprocessor System:
- 具有兩個以上的處理器(Processor)。
- 每個Processor擁有自己的CPU、Register、Cache。
- 可同時執行多個程序，提高系統效能。
- 但效能不是線性增加(有共享記憶體與同步的負擔)。
- 類型區分:
  1. 非對稱式多元處理(Asymmetric Mult...)
     - 主從架構(Master-Slave)
     - 特點: 由主處理器分配工作，其他從屬處理器執行。
  2. 對稱式多元處理(Symmetric Mult..., SMP)
     - 無主從架構
     - 特點: 所有處理器地位相同，共享記憶體。現代多核心CPU採用此架構。
## 多核心處理(Multicore System):
  - 一顆實體處理器中包含多個核心。
  - 每個核心可以獨立執行指令。
  - 各核心有自己的L1 cache，共享L2或L3。
- ### 多核心處理器與多處理器系統比較:
  - Multi-core: 一顆CPU裡多個core，在晶片內直接合作。
  - Multiprocesser: 多顆獨立CPU各自運作，透過主記憶體協同。
## Clustered System:
- 概念:
  - 由兩台或多台獨立電腦系統組成。
  - 透過區域網路(LAN)或高速連線(Interconnet)相互連結。
  - 共享儲存裝置(Storage-Area Network, SAN)。
  - 目的: 提供高可用性(High Availability, HA)及容錯能力(Fault Tolerance)。
- 類型:
  1. **非對稱式叢集(Asymmetric Clustering)**
     - 架構中有一台為Stand-by Server。
     - Stand-by平時不處理應用程式，僅監控工作中的主伺服器。
     - 若主伺服器故障，Stand-by立即接手運作，維持系統服務不中斷。
     - ### 優點: 架構簡單、容錯能力高。
     - ### 缺點: 待機主機大多閒置，資源利用率低。
  2. **對稱式叢集(Symmetric Clustering)**
     - 所有主機同時執行應用程式與監控任務。
     - 沒有閒置的Stand-by機器，每台都能運算。
     - 若某台故障，其他主機可立即分擔工作。
     - ### 優點: 資源利用率高、運算效能提升。
     - ### 缺點: 系統管理與同步較複雜，成本比較高。
## Graceful Degradation(優雅降級)
- 當多處理器或叢集系統中部份CPU故障時:
  -> 系統效能不會瞬間歸零，而是逐步下降。
- 表現出系統的高可用性與穩定性。
- 好處:
  - 提升服務連續性(Service Continuity)
  - 減少單點故障(Single Point of Failure, SPOF)
## 本總結:電腦系統架構依處理器數量與協作方式可分為單一處理器系統(Single-Processor System)、多處理器系統(Multiprocessor System)與叢集系統(Clustered System)。單一處理器系統僅有一個CPU核心，所有指令與任務都由同一顆核心負責，結構簡單但效能受限；多處理器系統則包含兩個以上的處理器，每個處理器擁有獨立的暫存器與快取，可同時執行多個程序以提升效能，常見架構包括主從式Asymmetric與對稱式(SMP)，其中現代多核心處理器屬於後者的延伸；而多核心系統(Multicore System)則是在單一晶片中整合多個核心，各核心共享部分快取並協同運作，達到高效能與節能平衡。進一步的叢集系統(Clustered System)由多台獨立電腦透過網路連結並共享儲存裝置形成，可提供高可用性與容錯能力，常見類型有非對稱式（以待命主機接手故障系統）與對稱式（所有主機共同運作）。此外，具備優雅降級(Graceful Degradation)能力的系統能在部分處理器或節點故障時持續運行，僅降低效能而不導致全面停擺，確保系統穩定與服務不中斷。

# 作業系統的運作
- ## 啟動與運作方式
  - OS是event-driven(事件驅動)系統: 只當事件發生時，作業系統才會被觸發執行。
  - 系統在待機時主要由應用程式或硬體運作，當事件出現才交回控制權給OS。
- ## Event的種類
  1. **Interrupt(硬體觸發)**
     - 當外部裝置完成任務時，會向CPU發出中斷訊號。
     - 流程:
       1. CPU暫停目前正在執行程式。
       2. 進入中斷服務程序(ISR)
       3. 作業系統處理該硬體事件後，返回原程式繼續執行。
  2. **Trap(or Exception)(軟體觸發)**
     1. Error Trap(例外錯誤):
        - 程式執行中出現異常，如A/0。
        - CPU每執行一條指令會檢查是否合法，若出現異常，產生Trap通知OS處理。
        - 由OS介入，確保系統穩定(如終止錯誤程式)。
     2. System Call(系統呼叫):
        - 程式主動向OS請求服務。
        - 屬於受控的Trap，用來執行系統功能。
        - 如:read() -> 從檔案讀取資料
- ## 整體運作流程
  - Hardware事件 -> Interrupt -> 由OS處理
  - Software錯誤 -> Trap(Error) -> 由OS保護系統穩定
  - 程式請求服務 -> Trap(System call) -> 由OS執行I/O或管理任務
- ### 小總結: OS只在事件發生時才"介入"，平時讓CPU執行一般程式。是以事件觸發點的控制系統。
- ## 程式設計模式演進
  1. **單一程式(Single-programming system)**
     - 同一時間僅允許一個程式(P1)執行。
     - 若P1需等待I/O，CPU會閒置(Idle)
     - 缺點: CPU利用率低。
  2. **多元程式(Multi-programming system)**
     - 多個程式同時存在主記憶體，CPU依需求切換執行。
     - 目的: 讓CPU永不閒置(Maximize CPU utilization)。
     - 作法: 透過CPU排程實現。
  3. **多個Process的執行方式**
     - 並行(Concurrent):
       - 說明: 單顆CPU在多個程式間"快速切換"，使人感覺同時進行。
       - 需求: 一顆CPU即可完成(多次切換)。
       - 對應系統: 多元程式系統。
     - 平行(Parallel):
       - 說明: 多顆CPU同時執行多個程式。
       - 需求: 需要多處理器或多核心系統。
       - 對應系統: 多處理器系統。
  4. **多個Process如何"同時執行"**
     - CPU Scheduling: 決定哪個Process先執行。
     - Job Scheduling: 決定哪些作業可被載入記憶體執行。
     - Memory Management: 管理多程式在主記憶體中的配置與釋放。
     - ### 三者協作讓多程式系統能同時進行而不互相衝突。
  5. **多元程式的限制與改進**
     - 若某程式(P1)長時間執行、不進行I/O，則其他程式(P2)需長時間等待:
       - 問題: Response Time過長(反應時間太慢)。
     - 改進方式:
       - 分時系統(Time-sharing System)
       - 讓CPU按時間片輪流分配資源，提升互動性與回應速度。
  6. **多任務系統(Time-sharing/Multi-tasking System)**
     - 目的: 解決多程式長時間占用CPU的問題。
     - 原理:
       - 將CPU時間分割成多個時間片(Time slice)。
       - 每個process只能使用固定時間，然後CPU會切換到下一個程式。
       - 提升系統反應速度(Response Time)，提供互動式(Interactive)體驗。
     - 長時間占用CPU的問題:
       - 若某程式陷入無限Loop或不釋放CPU，則其他程式會被阻塞。
       - 解決方式: 讓OS能主動奪回控制權。
     - 事件驅動的OS(Event-Driven OS)
       - OS只有在事件發生時才執行。
       - 進入OS的三種情境:
         1. Interrupt
         2. Trap(Error)
         3. Trap(System call)
     - 計時器(Timer)(OS奪回CPU之關鍵)
       - 硬體產生週期性中斷(Timer Interrupt)
       - 當時間片用完時，CPU會觸發中斷訊號 >> 進入OS >> 檢查是否該切換程式。
       - 若程式執行過久，OS會:
         1. 強制中止該程式。
         2. 將CPU分配給其他程式。
       - 為時間片輪轉(Round-Robin Scheduling)的基礎。
     - 雙模式操作(Dual-Mode Operation)
       - 為了防止使用者程式干擾系統或硬體，CPU設有兩種模式:
         1. User Mode:   權限:低  特點:只執行安全指令，不可控制硬體。
         2. Kernel Mode: 權限:高  特點:作業系統運行模式，可存取硬體與系統資源。
       - 透過暫存器的mode bit控制(1=user / 0=kernel)
     - 特權指令(Privileged Instructions)
       - 某些高風險指令僅能在Kernel中執行。
       - 如:關閉中斷(CLI)、清除記憶體、切換模式、寫入監控記憶體
       - 若使用者在user mode嘗試執行 -> 會產生Trap(Exception) -> 交由OS處理。
     - 模式切換的運作
       - 進入OS(User -> Kernel):
         - 發生中斷或系統呼叫時，mode bit變成0。
         - CPU跳到OS中的系統呼叫處理程式。
       - 返回使用者程式(Kernel -> User):
         - OS處理完後執行return from system call，mode bit恢復為1。
- ### 小總結: OS的核心職責是控制CPU時間的分配與保護系統安全。透過多元程式->分時系統->多任務->雙模式保護的演進，現代作業系統能同時運行多程式，維持高效且穩定的運作，並確保使用者程式不會直接影響核心與硬體。
## 本總結: 作業系統是以事件驅動(Event-Driven)為核心的控制系統，僅在中斷或軟體事件發生時介入運作，平時讓CPU執行一般程式。當硬體事件發生時由Interrupt觸發；當程式出錯或主動請求服務時，則由Trap(Exception/System Call)啟動，使OS能即時處理外部事件、維持系統穩定。隨著運算需求成長，系統從單一程式(Single-programming)發展為多元程式(Multiprogramming)，透過排程(Scheduling)機制在多個程式間切換執行，提升CPU利用率。進一步的分時系統(Time-sharing)將CPU時間切成多個時間片，使多個使用者或程式能輪流使用資源，形成現代的多任務系統(Multi-tasking System)。為防止程式無限佔用CPU，硬體計時器會定期產生週期性中斷(Timer Interrupt)，讓OS奪回控制權並重新分配執行。另一方面，CPU具備雙模式(Dual-Mode Operation)設計，以區分User Mode受限權限與Kernel Mode，並限制高風險的特權指令(Privileged Instructions)僅在核心模式執行，確保系統安全與穩定。整體而言，作業系統透過中斷、排程、時間片與雙模式等機制協同運作，使電腦能同時執行多項任務，並在效率、互動性與安全性間取得平衡。

# 作業系統的資源管理(Resource Management)
- 作業系統的核心任務之一有效管理系統資源，確保多個使用者與程式能公平、安全且高效地共享CPU、記憶體與I/O裝置。
- 主要包含以下六面向:
  1. **Process Management(行程管理)**
     - 概念:
       Process是正在執行的程式(Program)，而程式本身只是靜態存在於儲存裝置的可執行檔。
       Program -> 存在硬碟中，屬於靜態。
       Process -> 載入記憶體後執行，屬於動態實體。
     - **OS任務**:
       1. 建立與刪除使用者或系統行程。
       2. 暫停與恢復行程的執行。
       3. 行程間的同步(Synchronization)。
       4. 行程間的通訊(Communication)。
       5. 死結(Deadlock)的偵測與處理。
  3. **Memory Management(記憶體管理)**
     - 概念:
       記憶體是程式執行時的主要儲存空間，所有指令與資料都必須被載入記憶體後才能由CPU執行。
       一條記憶體條由多個IC組成，總容量=各IC容量總和。
       每個Address對應一個Byte的內容。
     - **記憶體的分配與結構**
       每個程式載入記憶體後會佔據不同區塊，依程式執行特性劃分:
         1. text:  內容:程式碼區               特性:儲存可執行指令
         2. data:  內容:已初始化的全域變數      特性:程式啟動時載入
         3. bss:   內容:未初始化的全域變數      特性:系統自動設為0
         4. heap:  內容:動態配置變數           特性:執行時由OS配置與釋放
         5. stack: 內容:區域變數與函式呼叫資訊  特性:LIFO結構，隨函式進出而變化
         6. system/kernel space: 內容:系統專用區 特性:儲存OS核心程式
     - **OS任務**
       1. 決定哪些資料在記憶體中、何時載入或釋放。
       2. 追蹤記憶體使用狀況，確認哪個區塊被哪個行程占用。
       3. 決定哪些程式或資料須移入或移出記憶體以提升效能。
       4. 動態進行記憶體的配置與回收(allocation/deallocation)。
     - **記憶體不足**
       記憶體空間不足時，OS透過:
       - 分頁(Paging)或分段(Segmentation)技術分配記憶體。
       - 虛擬記憶體(Virtual Memory): 利用硬碟暫時模擬主記憶體。
     - **Caching(快取)**
       - **背景與問題**
         當CPU讀取資料時，必須先從硬碟載入到主記憶體，再由主記憶體傳入CPU的register。
         CPU與main memory速度差距極大；CPU常需等待資料載入，造成處理效率下降。
       - **快取記憶體的設計原理**
         為了彌合速度差距，在CPU與main memory之間加入Cache Memory。
         Cache是一種容量小但速度極快的記憶體，用來暫存常用資料。
         當CPU要取資料時:
         - 先檢查Cache中是否已有該資料(Cache hit)，若有則直接使用。
         - 若沒有(Cache miss)，再從主記憶體載入，並存入Cache以便下次快速使用。
       - **多層快取(Multi-level Cache)**
         為了進一步提升效能，現代CPU採用多層結構: L1 >> L2 >> L3 >> main memory。
       - **快取的重要性**
         快取是速度與成本之間的折衷方案，在不顯著增加成本的前提下，大幅提升系統效能。
         它降低了CPU等待主記憶體資料的時間，讓指令執行更連續。
  4. **File-system Management(檔案系統管理)**
     - 概念:
       檔案系統是一種儲存與組織電腦資料的方法，讓使用者能方便地儲存、存取與搜尋資料。
     - **OS任務**
       1. 建立與刪除檔案與目錄
       2. 操作檔案與目錄的基本指令集(Primitives)
       3. 將檔案對應到次級儲存裝置(Mapping)
       4. 備份與穩定儲存(Backup)
  5. **Mass-storage Management**
     - 概念:
       主記憶體容量有限，電腦系統會利用輔助儲存裝置(Secondary Storage)來支援主記憶體的運作。
## 本總結: 作業系統的資源管理(Resource Management)是整個系統運作的核心任務，目的在於讓多個使用者與程式能公平、高效、安全地共享有限的硬體資源。其中，OS透過不同子系統協調各種資源：在Process Management中，OS負責建立、排程與同步多個行程，確保CPU能有效運作並避免死結；在Memory Management中，OS掌控主記憶體的分配與回收，決定資料的載入與釋放，並藉由快取(Caching)與虛擬記憶體(Virtual Memory)技術縮短CPU與記憶體的速度差、提升系統效能；File-system Management則提供有組織的資料儲存與存取方式，讓使用者能以檔案與目錄的形式管理資料；而Mass-storage Management負責管理輔助儲存裝置，如磁碟分割、排程與空間分配，確保長期資料儲存的穩定與安全。整體而言，作業系統的資源管理是銜接硬體與使用者之間的橋樑，透過行程、記憶體、檔案與儲存等多層機制，達到效能最大化與系統穩定性的平衡。

# 安全與保護
- Protection(保護):
  - 防止使用者惡意或誤操作破壞系統資源。
  - 確保各行程都在合法範圍內存取資源。
- Security(安全):
  - 防止未授權的存取、破壞、修改資料。
  - 確保資料的機密性、完整性與可用性。
# 虛擬化
# 分散式系統
# 核心資料結構
