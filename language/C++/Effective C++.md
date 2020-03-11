---
title: Effective C++
tags: C++ language, advanced 
notebook: C++
---

<!-- TOC -->

- [Effective C++](#effective-c)
  - [前言:](#前言)
  - [Chapter 1：讓自己習慣C++](#chapter-1讓自己習慣c)
    - [條款1：視C++為一個語言聯邦](#條款1視c為一個語言聯邦)
    - [條款2：盡量用const, enum, inline 取代#define](#條款2盡量用const-enum-inline-取代define)
    - [條款3：盡可能使用const](#條款3盡可能使用const)
    - [條款4：確定物件被使用前已先被初始化](#條款4確定物件被使用前已先被初始化)
  - [Chapter 2：構造/析構/賦值運算](#chapter-2構造析構賦值運算)
    - [條款5：了解C++默默編寫並調用那些函數](#條款5了解c默默編寫並調用那些函數)
    - [條款6：若不想使用編譯器自動生成的函數，就該明確拒絕](#條款6若不想使用編譯器自動生成的函數就該明確拒絕)
    - [條款7：為重載基類聲明virtual析構函數](#條款7為重載基類聲明virtual析構函數)
    - [條款8：別讓異常逃離析構函數](#條款8別讓異常逃離析構函數)
    - [條款9：絕不在構造和析構函數過程中調用virtual函數](#條款9絕不在構造和析構函數過程中調用virtual函數)

<!-- /TOC -->


# Effective C++
## 前言:
**被聲明explicit的構造函數通常比non-explici更受歡迎，除非有一個好理由允許構造函數被用於隱式類型轉換，否則explicit唯一宣告，因為它們禁止編譯器執行非預期的類型轉換**
## Chapter 1：讓自己習慣C++
### 條款1：視C++為一個語言聯邦

> 1. 以C為基礎。區塊(block)，語句(statments)，預處理器(preprocessor)，內置數據類型(build-in data types)，數組(arrays)，指針(pointer)，等等...
> 2. Object-Oriented C++。這部分也就是C with Classes所訴求的:classes，封裝，繼承，重載，virtual函數...
> 3. Template C++。泛型程式(generic programming)部分
> 4. STL是一個template library


**請記住：**
- C++高效編程守則視狀況而變化，取決於你用C++的哪個部分

### 條款2：盡量用const, enum, inline 取代#define

- 定義常量指針(constant pointers): const char* const authorName = "Scott Meyers"

```cpp
int main() {  
  int a = 100, b = 200;  // 常量指針  
  const int *pConstPointer;
  pConstPointer = &a;  
  cout << *pConstPointer << endl; // 讀權限  
  //(*pConstPointer)++; // 寫權限 錯誤：不能給常量賦值  
  pConstPointer = &b; // 修改指針指向的物件  
  /*總結：常量指針可以修改指向的物件，但只有讀權限，而沒有寫權限*/  
  // 指針常量  
  int* const pPointerConst = &a; // 指針常量必須定義時初始化
  cout << *pPointerConst << endl; // 讀權限  
  (*pPointerConst)++; // 寫權限  
  //pPointerConst = &b; // 指針常量不能修改指向的物件  
  /*總結：指針常量擁有讀、寫權限，但不可以修改指針指向的物件*/  
  // 指向常量的指針常量  
  const int* const pConstPointerConst = &a; // 定義指向常量的指針常量時必須初始化  
  cout << *pConstPointerConst << endl; // 讀權限 
  //(*pConstPointerConst)--; // 寫權限  
  //pConstPointerConst = &b; // 不能修改指向的物件  
  /*總結：指向常量的指針常量既不可以修改指向的物件，也沒有對物件的寫權限，只有讀權限*/  
  getchar();  
  return 0;
}
```

- **class專屬常量，為了將常量的作用域(scope)限制於classs內，你必須讓他成為class的成員(member)，而為了確保此常量至多只有一份實體，則需使用static成員；**

```cpp
class GamePlayer{
 private:
  static const int numTurn = 5;
  int score[numTurn];
  .......
}
```

有些編譯器可能不支持，將初值設定移至定義式

```cpp
class GamePlayer{
 private:
  static const int numTurn;
  .....
};
const int numTurn = 5;
}
```
或使用**the enum hack**

```cpp
class GamePlayer{
 private:
  enum { numTurn = 5 };
  int score[numTurn];
  .......
}
```

**請記住：**
- 對於單純常量，最好以const物件或enum替換#defines。
- 對於形似函數的宏，最好改用inline取代#defines。


### 條款3：盡可能使用const
```cpp
char greeting[] = "Hello";
char* p = greeting; //non-const pointer, non-const data
const char* p = greeting; //non-const pointer, const data
char* const p = greeting; //const pointer, non-const data
const char* const p = greeting; //const pointer, const data

std::vector<int> vec;
...
const std::vector<int>::iterator iter = vec.begin;
*iter = 10;  //沒問題，改變iter所指物
++iter;  //錯誤，iter是const
std::vector<int>::const_iterator cIter = vec.begin;
*cIter = 10;  //錯誤，*cIter是const
++cIter;  //沒問題，改變cIter
```

- const實施於成員函數的目的有二:
1. 使class接口較容易被理解
2. 使"操作const物件"成為可能

- bitwise constness(physical constness): 成員函數只有在不更改物件之任何成員變量(static除外)時才可以說是const。
- logical constness: 一個const成員函數可以修改它所處理的物件內的某些bits，但只有在客戶端偵測不出的情況下得如此。

**請記住：**
- 將某些東西聲明為const可幫助編譯器偵測出錯誤用法。const 可被施加於任何作用域內的物件、函數參數、函數返回類型、成員函數本體
- 編譯器強制實施Wise Constness，但你編程時應該使用"概念上的常量性"(Conceptual Costness)
- 當const和non-const成員函數有著實質等價的實現時，令non-const版本調用const版本可避免代碼重複

### 條款4：確定物件被使用前已先被初始化
- 讀取未初始化的值會導致不明確的行為。
- 永遠在使用對象之前先將它初始化。
```cpp
class PhoneNumber{...};
class ABEntry{
 public:
  ABEntry(const std::string& name, const std::string& address, const 
          std::list<PhoneNumber>&phones);
 private:
  std::string name;
  std::string address;
  std::list<PhoneNumber> thePhones;
  int numTimesConsulted;
};
ABEntry::ABEntry(const std::string& name, const std::string& address, const 
                 std::list<PhoneNumber>&phones){
  theName = name;  //這些都是賦值(assignments)而非初始化(initial)
  theAddress = address;
  thePhones = phones;
  numTimeConsulted = 0;
}
```
這會導致ABEntry物件帶有你的期望的值，但不是最佳做法。
ABEntry構造函數較佳寫法為：
```cpp
ABEntry::ABEntry(const std::string& name, const std::string& address, const 
                 std::list<PhoneNumber>&phones)
  :theName(name), 
  :theAddress(address),
  :thePhones(phones),
  :numTimeConsulted(0)
{}
//同理想要一個default構造
ABEntry::ABEntry()
  :theName(), 
  :theAddress(),
  :thePhones(),
  :numTimeConsulted()
{}
```
**為了避免需要記住成員變量何時須在成員初始列中初始化，何時不需要，最簡單的做法為：總是使用成員初值列。**

如果某編譯單元內的non-local static物件的初始化動作使用了另一個編譯單元內的某個non-local static物件，它所使用到的這個物件可能尚未初始化，因為C++對"定義於不同編譯單元內的non-local static物件"的初始化次序並無明確定義
**EX:**
```cpp
class FileSystem{  //來自你的程序庫
 public:
  ...
  std::size_t numDisks() const;  //眾多成員函數之一
  ...
};
extern FileSystem tfs;  //預備給客戶使用的物件

class Directory{  //由程序庫客戶建立
 public:
  Directory(params);
  ...
};
Directory::Directory(params){
  ...
  std::size_t disks = tfs.numDisks();  //使用tfs物件
}

Directory tempDir(params);  //客戶主程序宣告
```
初始化次序的重要性可以看出來；除非tfs在tempDir之前被初始化，否則tempDir的構造函數會用到尚未初始化的tfs。多個編譯單元內的non-local static物件經由"模板隱式具現化, implicit template instantiations"形成
**用local static物件取代non-local static物件** 
```cpp
class FileSystem{...};  //同前
FileSystem& tfs(){  //這個函數用來替代tfs物件，它在FileSystem class中可能是個static。
  static FileSystem fs;  //定義並初始化一個local static物件
  return fs;  //返回一個reference指向上述物件
}
class Directory{...};  //同前
Directory::Directory(params){  //同前，但原本的reference to tfs 改為 tfs()
  ...
  std::size_t disks = tfs().numDisks();
  ...
}
Directory& tempDir(){  //這個函數用來替代tempDir物件
  static Directory td;  //定義並初始化一個local static物件
  return td;  //返回一個reference指向上述物件
} 	
```
這樣修改過後使用`tfs()` 和 `tempDir()`而不再是`tfs`和`tempDir`。也就是說他們使用函數返回的"指向static物件"，而不再是static物件自身。
**任何一種non-const static物件，不論他是local或non-local，在多線程環境下"等待某事發生"都會有麻煩。**
**請記住：**
- 為內置型物件進行手動初始化，因為C++不保證初始化它們
- 構造函數最好使用成員初始列(member initialization list)，而不要在構造函數本體內使用賦值操作(assignment)。初始列列出成員變量，其排列次序應該和它們在class中的聲明**次序**相同
- 為免除"跨編譯單元之初始化次序"問題，請以local static物件替換non-local static物件

## Chapter 2：構造/析構/賦值運算
你寫的每個class都會有一個或多個constructor，一個destructor，一個copy assignment操作符，本章提供的引導可讓你把這些函數良好的集結在一起，形成class的梁柱
### 條款5：了解C++默默編寫並調用那些函數
```cpp
class Empty{};
//等同於編譯器為你聲明default構造函數
class Empty{
 public:
  Empty(){...}
  Empty(const Empty& rhs){...}
  ~Empty(){...}

  Empty& operator=(const Empty& rhs){...} 
};
```
**請記住：**
- 編譯器可以暗自為class創建`default`構造函數、`copy`構造函數、`copy assignment`操作符和析構函數

### 條款6：若不想使用編譯器自動生成的函數，就該明確拒絕
```cpp
class Uncopyable{
 protected:
  Uncopyable();  //允許derived物件構造及析構
  ~Uncopyable();
 private:
  Uncopyable(const Uncopyable);  //但阻止copying
  Uncopyable& operator=(const Uncopyable&);
}
class HomeForSale: private Uncopyable{...};  //class 不再聲明copy構造函數或copy assignment操作符
```
**請記住：**
- 為駁回編譯器自動(暗自)提供的功能，可將相應的成員函數聲明為private並且不予實現。使用像Uncopyable這樣的base class也是一種做法

### 條款7：為重載基類聲明virtual析構函數
有
```cpp
class TimeKeeper{
 public:
  TimeKeeper();
  ~TimeKeeper();
  ...
};
class AtomicClock:public TimeKeeper{...};
class WaterClock:public TimeKeeper{...};
class WrisktClock:public TimeKeeper{...};
```
上述設計代碼為`factory(工廠)`函數，返回一個指針物件。Factory函數會"返回一個base class指針，指向新生成之derived class物件":

```cpp
TimeKeepeer* getTimeKeeper();  //返回一個指針,指向一個TimeKeeper派生類的動態分配物件
```
為了遵守`factory(工廠函數)`的規矩，被getTimeKeeper()返回的物件必須位於heap。
```cpp
TimeKeepeer* ptk = getTimeKeeper();
...
delete ptk;
```
> 依賴客戶執行delete動作，基本上便帶有某種錯誤的傾向
問題出在於`getTimeKeeper()`返回的指針指向一個derived class物件(例如: AtomicClock)，而那個物件卻經由一個base class指針(例如: TimeKeepeer*
指針)被刪除，而目前的base class(TimeKeeper)有個non-virtual析構函數。

消除"局部銷毀"這個問題很簡單:
**給base class一個virtual析構函數。此後刪除derived class物件時，就會銷毀所有物件，包括derived class**
```cpp
class TimeKeeper{
 public:*
  TimeKeeper();
  virtual ~TimeKeeper();
  ...
};
TimeKeeper* tpk = getTimeKeeper();
...
delete tpk;
```
> 像TimeKeeper這樣的base class析構函數外通常還有其他virtual函數，因為virtual函數的目的是允許derived class的實現客製化，在不同derived class中有不同的實現代碼。
> 反之可以說如果class中不含virtual函數，通常表示它並不意圖作為一個base class

**無端的將所有class的析構函數宣告為virtual，就像從未宣告它們為virtual一樣，都是錯誤的**

```cpp
class SpecialString:public std::string{  //餿主意，std::string有個non-virtual析構函數
...
}
SpecialString* pss = new SpecialString("Impending Doom");
std::string* ps;
...
ps = pss;
...
delete ps;  //未有定義，現實中的*SpecialString資料會洩漏，因為SpecialString析構函數未定義
```
上述代碼乍看無害，但如果你在程序某處無意間將一個pointer-to-SpecialString轉換為一個pointer-to-string，然後將string delete掉，會被流放到"行為不明確"的惡地上。

**如果曾經企圖繼承一個帶有non-virtual析構函數的class，拒絕誘惑吧!**

**請記住**
- polymorphic(帶多態性質的) base classes應該聲明應該聲明一個virtual析構函數。如果class帶有任何virtual任何virtual函數，那麼它就應該擁有一個virtual析構函數。
- Classes的設計目的如果不是作為base classes使用，或不是具備多態性(polymorphically)，就不該聲明virtual析構函數

### 條款8：別讓異常逃離析構函數
```cpp
class Widget{
 public:
  ...
  ~Widget(){...}  //假設這裡可能吐出一個異常
};
void doSomething(){
  std::vector<Widget> v;
  ...
}  //v在這裡被自動銷毀
```
只要析構函數吐出異常，即使並非使用容器或array，程序也可能過早結束或出現不明確行為。在看個例子:
```cpp
class DBConnection{
 public:
  ...
  static DBConnection create();  //這個函數返回DBConnection物件。
  ...
  void close();  //關閉聯機;失敗拋出異常。
}
class DBConn{  //這個class用來管理DBConnection物件
 public:
  ...
  ~DBConn(){  //確保數據庫連接總是會被關閉
    db.close();
  }
 private:
  DBConnection db;
};
```
會允許客戶寫出這樣的代碼:
```cpp
{
  DBConn dbc(DBConnection::create());  //建立物件並交給DBConn物件以便管理
  ...
}  //在區塊結束點DBConn物件被銷毀，因而自動為DBConnection物件調用close()
```
如果調用導致異常DBConn析構函數會傳播該異常，也就是允許它離開這個析構函數 ，這些方法可以解決這一問題:
```cpp
//法一:
DBConn::~DBConn(){
  try {db.close();}
  catch (...){
    ...//製作運轉紀錄，記下對close調用失敗;
    std::abort();
  }
}
//法二:
DBConn::~DBConn(){
  try {db.close();}
  catch(...) {
    ...//製作運轉紀錄，記下對close調用失敗;
  }
}
//最佳解:
class DBConn{
 public:
  ...
  void close(){
    db.close();
    closed = true;
  }
  ~DBConn(){
    if(!closed) {
      try {db.close();}
      catch(...) {
        ...//製作運轉紀錄，記下對close調用失敗;
      }
    }
  }
 private:
  DBConnection db;
  bool closed;
}
```
**請記住**
- 析構函數絕對不要吐異常。如果一個被析構函數調用的函數可能拋出異常，析構函數應捕捉任何異常然後吞下它們或結束程序。
- 如果客戶需要對某個操作函數運行期間拋出異常做出反應，那麼class應該提供一個普通函數執行該操作(而非析構函數內)。

### 條款9：絕不在構造和析構函數過程中調用virtual函數
