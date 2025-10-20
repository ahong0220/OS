# Process Concept(行程概念)
## What is Process?
- Process: 正在執行中program，也可稱為job或task。
- Program是靜態的(存在硬體中)；process是動態的(載入記憶體後執行)。
- 一個程式可對應多個獨立的行程。
## Process need resource(行程需要的資源)
- 行程執行時需由OS分配資源，如:cpu、memory、I/O。
- 資源透過system bus與controller連結。
- 若無法取得必要資源，行程無法執行。
## Process in memory
1. **程式在記憶體中的區域分布**
   - 行程在執行時會被分為四個主要區段(由低位址到高位址):
   - <img width="867" height="486" alt="image" src="https://github.com/user-attachments/assets/6aedcf18-5edf-4a45-911f-a29044088a3b" />
2. **Text section(程式代碼區)**
   - 儲存編譯後的機器碼指令。
   - 從原始碼 -> 組合語言 -> 機器碼 -> 載入至Text區。
3. **Data section(全域變數區)**
   - 儲存global/static變數。
   - 在程式執行期間值保持不變。
   - 區分已初始化(initialized)與未初始化(uninitialized)資料。
4. **Stack section(堆疊區)**
   - 儲存:區域變數(local variables)、函式參數(function parameters)、回傳位址(return address)
   - 遵循LIFO(後進先出): 呼叫函式時push，返回時pop。
   - 由作業系統自動分配與回收。
5. **Heap section(堆區)**
   - 儲存動態分配的記憶體。
   - 由程式員手動配置與釋放。
   - 空間可在執行中變化。
### 總結: Text與Data區大小固定；Heap向上成長、Stack向下成長；中間區域為彈性空間，確保行程可動態使用記憶體。
## Process State(行程狀態)
- **New**: 行程正在建立中。
- **Ready**: 行程已在記憶體中等待CPU排程。
- **Running**: 行程正在被CPU執行(每個CPU同時僅有一個process處於此狀態)。
- **Waiting(Blocked)**: 行程暫停等待事件(I/O完成、訊號等)。
- **Terminated**: 行程執行結束，等待系統回收資源。
- <img width="727" height="297" alt="image" src="https://github.com/user-attachments/assets/e5baab60-868e-4989-9905-a6077c2c82f2" />
## Process Contorl Block(PCB, 行程控制區塊)
- **功能**: PCB是OS為每個process維護的一份資料結構，當行程被中斷時，CPU會把該行程的所有狀態資訊暫存到PCB中，當行程再次被排程執行時，CPU從PCB取回狀態繼續執行。
- **內容**:
  1. Process state: 當前狀態(new, ready, running...)
  2. Program counter: 下一條要執行的指令位址
  3. CPU registers: 暫存器內容(執行中資料、指令暫存)
  4. CPU scheduling info: 優先權、排程佇列指標等資訊
  5. Memory-management info: 該行程分配的記憶體範圍
  6. Accounting info: 使用的CPU時間、開始時間、限制等
  7. I/O status info: 已開啟的檔案與分配的IO裝置清單
## Threads(執行緒)
- 概念:
  - 傳統上一個process只有一個執行緒。
  - 若同一個process內有多個program counter，可同時執行不同區段 -> 稱為"多執行緒"。
- 優點:
  - 可同時處理多項任務。
  - 若所有工作都在單一thread，下載時整個畫面會卡住。
### 總結: PCB儲存行程執行狀態，確保中斷後能恢復。Thread是行程內的執行單位，允許同時進行多工作。
# Process Scheduling(行程排程)
**定義**
- process scheduler(CPU scheduler): 一段系統程式，負責從ready queue挑選下一個要執行的process。
- Degree of multiprogramming: 記憶體中同時存在的行程數量；越多表示越忙碌。
- I/O-bound process: 多數時間在執行I/O(如硬碟、網路)，計算少。
- CPU-bound process: 多數時間在做計算(數學、壓縮、挖礦)，I/O少。
## Scheduling Queues(排程佇列)
**定義**
- processes會在記憶體中排隊等待執行，這些排隊結構稱為scheduling queues。
- 由OS管理。
**兩種主要佇列**
1. Ready Queue(就緒佇列): 存放所有已在記憶體中、等待CPU執行的process。只有一個ready queue。
2. Wait Queue(等待佇列): 當process執行到一半需要I/O或等待事件時，會暫時釋放CPU並進入對應的wait queue。不同I/O裝置或事件各有自己的wait queue(可能有多個)。
**佇列的儲存方式**
- 系統以linklist將所有process的PCB串接起來。
- 每個queue都有head和tail指標指向隊首與隊尾。
- ready queue及多個wait queue都以相同方式串接。
**Process的流動**
- process可在不同queue間切換: ready queue > cpu > IO wait queue > ready queue
- 當process執行IO、時間片到期、或發生中斷時，都可能離開或返回ready queue。
## CPU Scheduling(CPU排程)
**定義**
- 是OS中的一段程式碼。
- 負責從ready queue中挑選一個process交給CPU執行。
- 運作在kernel mode，非使用者程式的一部份。
**觸發時機**
- 當timer中斷發生時。
- 或當: process結束執行；process進入waiting狀態；process被強制中斷(preemption)。
**運作過程**
1. CPU正在執行某程式。
2. timer interrupt發生。
3. 目前程式暫停，系統跳入ISR。
4. ISR呼叫CPU scheduler判斷。
5. 若需切換，執行context switch，載入下一個process的狀態繼續執行。
## Swapping(置換)
**當記憶體空間不足時，OS會暫時把某些process從記憶體移到硬碟。**
- 移出記憶體: swap out
- 移回記憶體: swap in
**目的**: 在有限記憶體下調整可同時執行的行程數量。
# Operations on Processes(行程操作)
# Interprocess Communication(行程間通訊)
# IPC in shared-memory systems(共享記憶體中的行程通訊)
# IPC in message-passing systems(訊息傳遞中的行程通訊)
