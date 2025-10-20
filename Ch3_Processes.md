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
### 整體概念: Text與Data區大小固定；Heap向上成長、Stack向下成長；中間區域為彈性空間，確保行程可動態使用記憶體。
## Process State(行程狀態)
- **New**: 行程正在建立中。
- **Ready**: 行程已在記憶體中等待CPU排程。
- **Running**: 行程正在被CPU執行(每個CPU同時僅有一個process處於此狀態)。
- **Waiting(Blocked)**: 行程暫停等待事件(I/O完成、訊號等)。
- **Terminated**: 行程執行結束，等待系統回收資源。
- <img width="727" height="297" alt="image" src="https://github.com/user-attachments/assets/e5baab60-868e-4989-9905-a6077c2c82f2" />

# Process Scheduling(行程排程)
# Operations on Processes(行程操作)
# Interprocess Communication(行程間通訊)
# IPC in shared-memory systems(共享記憶體中的行程通訊)
# IPC in message-passing systems(訊息傳遞中的行程通訊)
