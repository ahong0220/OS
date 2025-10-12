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
- CPU不直接控制裝置，而是透過控制器進行資料傳輸與命令執行。

## 裝置驅動程式(Device Driver):
- 屬於軟體，為作業系統的一部份。
- 每個控制器都有對應的驅動程式。
- 驅動程式負責將OS的高階命令轉換成控制器能理解的低階指令。

## 電腦系統運作的三個關鍵:
- 中斷(Interrupt): 讓CPU在執行指令時能被外部事件打斷並即時處理。
- 儲存體結構(Storage Structure): 確保資料能在CPU、記憶體與外部裝置間正確傳遞。
- I/O結構(Input/Output Structure): 管理裝置之間的資料交換與控制訊號。

## 1. 中斷(Interrupt):
- 定義: 中斷是硬體或軟體事件發生時，發送給CPU的訊號，用來要求暫停目前工作並優先處理該事件。
- 目的: 讓CPU不必一直等待裝置完成任務，提高效率；使系統能即時回應外部事件。
- 運作流程:
  1. Device Controller偵測到事件，發出IRQ(中斷請求)。
  2. CPU停止目前指令的執行，保存現有狀態。
  3. CPU透過中斷向量表(IVT)找出對應的中斷服務程式。
  4. OS中的IRS處理該事件(如讀取資料、重設狀態)。
  5. IRS結束，CPU恢復原先執行狀態並繼續原程式。
- 系統元件:
  - IRQ(Interrupt Request Line): 硬體線路，用來傳送中斷請求。
  - PIC(Programmable Interrupt Controller): 管理多個IRQ的輸入，負責排序、篩選並傳送中斷給CPU。
  - IVT(Interrupt Vector Table): 記錄各種中斷對應的IRS位址，供CPU查找。
  - ISR(Interrupt Service Routine): 實際的中斷處理程式，由OS執行。
- 重點概念:
  - 硬體中斷: 由外部裝置(如鍵盤、磁碟)產生。
  - 軟體中斷: 由程式主動觸發，用於系統呼叫或例外處理。
  - 優點: 提升系統反應速度與資源利用率。
- ### 總結: 中斷是讓CPU能即時回應外部事件、保持系統高效運作的核心機制。

## 2. 儲存體結構(Storage Structure):
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
- ### 總結: 儲存體結構是由多層次記憶體組成的階層系統，用以兼顧效能、容量與成本，使CPU能高效執行程式並穩定儲存資料。

## 3. I/O結構(Input/Output Structure):
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
- ### 總結: Polling是由CPU親自檢查，Interrupt是由裝置通知CPU，DMA是由裝置自己處理並告訴CPU"已完成"。

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
  1. 非對稱式叢集(Asymmetric Clustering):
     - 架構中有一台為Stand-by Server。
     - Stand-by平時不處理應用程式，僅監控工作中的主伺服器。
     - 若主伺服器故障，Stand-by立即接手運作，維持系統服務不中斷。
     - ### 優點: 架構簡單、容錯能力高。
     - ### 缺點: 待機主機大多閒置，資源利用率低。
  3. 對稱式叢集(Symmetric Clustering):
     - 所有主機同時執行應用程式與監控任務。
     - 沒有閒置的Stand-by機器，每台都能運算。
     - 若某台故障，其他主機可立即分擔工作。
     - ### 優點: 資源利用率高、運算效能提升。
     - ### 缺點: 系統管理與同步較複雜，成本比較高。
## Graceful Degradation(優雅降級):
- 當多處理器或叢集系統中部份CPU故障時:
  -> 系統效能不會瞬間歸零，而是逐步下降。
- 表現出系統的高可用性與穩定性。
- 好處:
  - 提升服務連續性(Service Continuity)
  - 減少單點故障(Single Point of Failure, SPOF)
