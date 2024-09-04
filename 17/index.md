# 抽象工厂模式的具体实现


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

### 1. **图形用户界面（GUI）组件**

- **场景**：创建不同操作系统的 GUI 组件，如按钮和文本框。

- 应用

  

  ```java
  interface Button {
      void paint();
  }
  
  interface TextBox {
      void display();
  }
  
  interface GUIFactory {
      Button createButton();
      TextBox createTextBox();
  }
  
  class WindowsButton implements Button {
      public void paint() {
          System.out.println("Windows Button");
      }
  }
  
  class WindowsTextBox implements TextBox {
      public void display() {
          System.out.println("Windows TextBox");
      }
  }
  
  class MacButton implements Button {
      public void paint() {
          System.out.println("Mac Button");
      }
  }
  
  class MacTextBox implements TextBox {
      public void display() {
          System.out.println("Mac TextBox");
      }
  }
  
  class WindowsFactory implements GUIFactory {
      public Button createButton() {
          return new WindowsButton();
      }
  
      public TextBox createTextBox() {
          return new WindowsTextBox();
      }
  }
  
  class MacFactory implements GUIFactory {
      public Button createButton() {
          return new MacButton();
      }
  
      public TextBox createTextBox() {
          return new MacTextBox();
      }
  }
  
  // 使用
  GUIFactory factory = new WindowsFactory();
  Button button = factory.createButton();
  TextBox textBox = factory.createTextBox();
  button.paint();
  textBox.display();
  ```

### 2. **数据库连接**

- **场景**：支持多种数据库（如 MySQL 和 PostgreSQL）。

- 应用

  ：

  java

  ```java
  interface Connection {
      void connect();
  }
  
  interface Command {
      void execute();
  }
  
  interface DatabaseFactory {
      Connection createConnection();
      Command createCommand();
  }
  
  class MySQLConnection implements Connection {
      public void connect() {
          System.out.println("Connected to MySQL");
      }
  }
  
  class MySQLCommand implements Command {
      public void execute() {
          System.out.println("Executing MySQL command");
      }
  }
  
  class PostgreSQLConnection implements Connection {
      public void connect() {
          System.out.println("Connected to PostgreSQL");
      }
  }
  
  class PostgreSQLCommand implements Command {
      public void execute() {
          System.out.println("Executing PostgreSQL command");
      }
  }
  
  class MySQLFactory implements DatabaseFactory {
      public Connection createConnection() {
          return new MySQLConnection();
      }
  
      public Command createCommand() {
          return new MySQLCommand();
      }
  }
  
  class PostgreSQLFactory implements DatabaseFactory {
      public Connection createConnection() {
          return new PostgreSQLConnection();
      }
  
      public Command createCommand() {
          return new PostgreSQLCommand();
      }
  }
  
  // 使用
  DatabaseFactory factory = new MySQLFactory();
  Connection connection = factory.createConnection();
  Command command = factory.createCommand();
  connection.connect();
  command.execute();
  ```

### 3. **汽车制造**

- **场景**：制造不同类型的汽车（如 SUV 和轿车）。

- 应用

  ：

  java

  ```java
  interface Engine {
      void start();
  }
  
  interface Tire {
      void roll();
  }
  
  interface CarFactory {
      Engine createEngine();
      Tire createTire();
  }
  
  class SUVEngine implements Engine {
      public void start() {
          System.out.println("Starting SUV engine");
      }
  }
  
  class SUVTire implements Tire {
      public void roll() {
          System.out.println("Rolling SUV tire");
      }
  }
  
  class SedanEngine implements Engine {
      public void start() {
          System.out.println("Starting Sedan engine");
      }
  }
  
  class SedanTire implements Tire {
      public void roll() {
          System.out.println("Rolling Sedan tire");
      }
  }
  
  class SUVFactory implements CarFactory {
      public Engine createEngine() {
          return new SUVEngine();
      }
  
      public Tire createTire() {
          return new SUVTire();
      }
  }
  
  class SedanFactory implements CarFactory {
      public Engine createEngine() {
          return new SedanEngine();
      }
  
      public Tire createTire() {
          return new SedanTire();
      }
  }
  
  // 使用
  CarFactory factory = new SUVFactory();
  Engine engine = factory.createEngine();
  Tire tire = factory.createTire();
  engine.start();
  tire.roll();
  ```

### 4. **报告生成**

- **场景**：生成不同格式的报告（如 PDF 和 Word）。

- 应用

  ：

  java

  ```java
  interface Report {
      void generate();
  }
  
  interface ReportFactory {
      Report createReport();
  }
  
  class PDFReport implements Report {
      public void generate() {
          System.out.println("Generating PDF report");
      }
  }
  
  class WordReport implements Report {
      public void generate() {
          System.out.println("Generating Word report");
      }
  }
  
  class PDFReportFactory implements ReportFactory {
      public Report createReport() {
          return new PDFReport();
      }
  }
  
  class WordReportFactory implements ReportFactory {
      public Report createReport() {
          return new WordReport();
      }
  }
  
  // 使用
  ReportFactory factory = new PDFReportFactory();
  Report report = factory.createReport();
  report.generate();
  ```
