アジャイル検定Lv.2 勉強メモ - デザインパターン
-----

# アジャイル検定Lv.2 出題範囲
- モデリング
  - オブジェクト指向設計：継承、インターフェース、ポリモーフィズム、疎結合、Dependency Injection
- コーディング
  - コーディングルール：ツールによる確認(checkstyle)
  - ペアプログラミング
  - リーダビリティ(コードの読みやすさ)
  - テストコード(Mock、Testing frameworkなど)
  - 静的解析ツール(SonarQube)
  - ドキュメンテーション
- 構成管理
  - チーム開発：SCM(ソースの変更管理システム)、分散型(git)、集中型(Subversion、CVS 等)
  - ブランチ戦略：ブランチとマージ、レビュー・受入(プルリクエスト)
  - コンテナ技術
- テスト
  - TDD:Junit(モックを使ったテスト、テスト結果レポートの見方、網羅率C0,C1,C2)
    品質管理のためのテスト(パフォーマンステスト、結合テスト、総合テスト・システムテスト)
    ユーザー受入テスト、ブラックボックステスト、ホワイトボックステスト
- 常時結合
  - 自動化の導入：何時動かして結果から何を読み取るか、自動化の導入効果、何を自動化するか(ビルド⇒テスト⇒デプロイ等)
  - 何のため、誰のために、常時結合(CI)をおこなうのか
- **デザインパターン**
  - **デザインパターンを使うことのメリット**
  - **ロバート・C.マーチン「アジャイルソフトウェア開発の奥義」(アジャイルな設計、単一責務、Open/Closedの法則)、GoFのデザインパターン、DI(Dependency Injection)**
  - **オブジェクト指向開発の考え方(継承、カプセル化、ポリモーフィズムなど)**
  - **デザインパターンを使うことのメリット(各パターンの利用法、メリット)**
  - **システムアーキテクチャ設計(拡張性、保守性)**
  - **UML(Unified Modeling Language)**
- リファクタリング
  - マーティン・ファウラー「リファクタリング」(コードの不吉な匂い等)
  - オブジェクト指向設計原則(Principles Of Object Oriented Design)
- チームのスキル
  - スプリント計画
  - 自己組織化されたチーム：メンバーの行動規範(コミュニケーション、自立と協調)
  - レトロスペクティブ(振り返り)


# デザインパターン

デザインパターン

- よく出会う問題とそれにうまく対処するためのソフトウェア設計をパターン化し、再利用しやすい形に整理したもの
- メリット
  - 再利用性の高い柔軟な設計ができる
  - 技術者どうしの意思疎通が容易になる

GoFのデザインパターン 

- 生成に関するパターン
  - Abstract Factory
  - Builder
  - Factory Method
  - Prototype
  - Singleton
- 構造に関するパターン
  - Adapter
  - Bridge
  - Composite
  - Decorator
  - Facade
  - Flyweight
  - Proxy
- 振る舞いに関するパターン
  - Chain of Responsibility
  - Command
  - Interpreter
  - Iterator
  - Mediator
  - Memento
  - Observer
  - State
  - Strategy
  - Template Method
  - Visitor

Abstract Factory

- 関連したオブジェクトを間違いなく生成するためのパターン

```typescript
interface Factory {
  getSoup(): object
  getVegetables(): object
}
class KimuchiFactory implements Factory {
  getSoup(): object { return 'キムチスープ' }
  getVegetables(): object { 'キムチ,しめじ' }
}
class Pot {
  addSoup(soup: object) { /* ... */ }
  addVegetables(vegetables: object) { /* ... */ }
}
function createFactory(type: string): Factory {
  if (type === 'キムチ鍋') {
    return new KimuchiFactory()
  } else {
    return /* ... */
  }
}
function main() {
  const factory = new createFactory('キムチ鍋')
  const pot = new Pot()
  pot.addSoup(factory.getSoup())
  pot.addVegetables(factory.getVegetables())
}
```

Builder

- オブジェクトの作成過程をコントロールするためのパターン

```typescript
interface Builder {
  makeTitle();
  makeString();
  close();
  getResult<T>(): T;
}
class Director {
  constructor(builder: Builder) {
    this.builder = buidler;
  }
  construct() {
    this.builder.makeTitle('場所');
    this.builder.makeString('東京都港区 1-1-1');
    this.builder.makeTitle('日時');
    this.builder.makeString('2999/01/23');
    close();
  }
}
function main() {
  const builder = new HtmlBuilder();
  const director = new Director(builder);
  director.construct();
  const html = builder.getResult<Html>();
}
```

Factory Method

- 柔軟にオブジェクトを生成するためのパターン

```typescript
abstract class Factory {
  abstract createAccount(name: string): Account;
  create(name: string): Account {
    const account = new createAccount(name)
    return account;
  }
}
interface Account {
  type: string;
  name: string;
}
class AdminFactory extends Factory {
    createAccount(name: string): Account {
      const account = new Account();
      account.type = 'admin';
      account.name = name;
      return account;
    }
}
function main() {
  const factory = new AdminFactory();
  const admin1 = factory.create('山田 太郎');
  const admin2 = factory.create('佐藤 次郎');
}
```

Prototype

- あらかじめ用意しておいた「原型」からインスタンスを生成するようにするためのパターン

```typescript
interface Clonable {
  createClone(): Clonable;
}
class Manager {
  hashMap: Map = new Map();
  register(name: string, cloneable: Clonable) {
     this.hashMap[name] = cloneable;
  }
  create(name: string): Clonable {
    return this.hashMap[name].createClone();
  }
}
function main() {
  const manager = new Manager();
  manager.register('class a', new ClassA());
  manager.register('class b', new ClassB());

  const a = manager.create('class a') as ClassA;
  const b = manager.create('class b') as ClassB;
}
```

Singleton

- インスタンスが1つだけ存在することを保証するためのパターン

```typescript
class Singleton {
  static singleton: Singletone = new Singletone();
  private constructor() {}
  getInstance() {
    return this.singleton;
  }
}
```

Adapter

- インタフェースに互換性の無いクラス同士を組み合わせることを目的としたパターン

```typescript
interface Student {
  getFullName(): string;
}
class Child {
    constructor(firstName: string, lastName: string) {
      this.firstName = firstName;
      this.lastName = lastName;
    }
}
// 継承を利用したパターン
class ChildAdapter extends Child implements Student {
  getFullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }
}
// 委譲を利用したパターン
class ChildAdapter implements Student {
  constructor(firstName: string, lastName: string) {
    this.child = new Child(firstName, lastName);
  }
  getFullName(): string {
    return `${this.child.firstName} ${this.child.lastName}`;
  }
}
function main() {
  const student = new ChildAdapter('Taro', 'Yamada')
  student.getFullName();
}
```

Bridge

- 機能を拡張するクラスと実装を拡張するクラスを分けるためのパターン

```typescript
class Sorter {
  constructor(sortImpl: SortImpl) {
    this.sortImpl = sortImpl
  }
  sort(array: Array) {
    this.sortImpl.sort(array);
  }
}
class ImmutableSorter {
  immutableSort(array: Array): Array {
    const clonedArray = array.clone();
    return this.sortImple.sort(clonedArray);
  }
}
interface SortImpl {
  sort();
}
class QuickSortImpl implements SortImpl {
  sort() { /* クイックソート処理 */ }
}
class BubbleSortImpl implements BubbleSortImpl {
  sort() { /* バブルソート処理 */ }
}
function main() {
  new Sorter(new QuickSortImpl()).sort([5,1,2])
  new Sorter(new BubbleSortImpl()).sort([5,1,2])

  const arr1 = new ImmutableSorter(new QuickSortImpl()).immutableSort([5,1,2])
  const arr2 = new ImmutableSorter(new BubbleSortImpl()).immutableSort([5,1,2])
}
```

Composite

- 容器と中身を同一視することで再帰的な構造を扱いやすくするためのパターン

```typescript
interface Entry {
  getName(): string;
  remove();
}
class File implements Entry {
  constructor(name: string) { this.name = name }
  getName(): string { return this.name }
  remove() { console.log(`${this.getName()}を削除しました`) }
}
class Directory implements Entry {
  constructor(name: string) {
    this.name = name
    this.entries = []
  }
  getName(): string { return this.name }
  remove() {
    for (const entry of this.entries) {
      entry.remove()
    }
    console.log(`${this.getName()}を削除しました`)
  }
  add(entry: Entry) {
    this.entries.push(entry)
  }
}
function main() {
  const rootDir = new Directory('root')
  const homeDir = new Directory('home')

  homeDir.add(new File('.bashrc'))
  rootDir.add(homeDir)
  rootDir.remove()
}
```

Decorator

- 飾り枠と中身を同一視することで、柔軟に機能拡張しやすくするためのパターン

```typescript
interface Ice {
  getName(): string
}
class VanillaIce implements Ice {
  getName(): string { return 'バニラアイス' }
}
class NutsToppingIce implements Ice {
  constructor(ice: Ice) { this.ice = ice }
  getName(): string {
    return `ナッツ ${this.ice.getName()}`
  }
}
class ChocoToppingIce implements Ice {
  constructor(ice: Ice) { this.ice = ice }
  getName(): string {
    return `チョコ ${this.ice.getName()}`
  }
}
function main() {
  new VanillaIce().getName() // バニラアイス
  new NutsToppingIce(new VanillaIce()).getName() // ナッツ バニラアイス
  new ChocoToppingIce(new NutsToppingIce(new VanillaIce())).getName() // チョコ ナッツ バニラアイス
}
```

Facade

- 複数クラス組み合わせて行う処理を、窓口となるクラスを作ってシンプルに利用できるようにするためのパターン

```typescript
  class HtmlWriter {
    setTitle(title: string) { this.title = title }
    setBody(body: string) { this.body = body }
    output(): string {
      return `<html>
                <head>${this.title}</head>
                <body>${this.body}</body>
              </html>`
    }
  }
  class FileWriter {
    create(file: string, content: string) { /* ファイル書き込み処理 */ }
  }
  class PageMaker {
    makeWelcomePage(title: string, file: string) {
      const htmlWriter = new HtmlWriter()
      htmlWriter.setTitle(title)
      htmlWriter.setBody(`ようこそ ${title} へ!!`)

      const fileWriter = new FileWriter()
      fileWriter.create(file, fileWriter.output())
    }
  }
  function main() {
    const pageMaker = new PageMaker()
    pageMaker.makeWelcomePage('ABC', 'welcome.html')
  }
```

Flyweight

- インスタンスを共有することでリソースを無駄なく使うためのパターン

```typescript
class Stamp {
  constructor(value: string) { this.value = value }
  print() { console.log(this.value) }
}
class StampFactory {
  stamps = {}
  get(value: string): Stamp {
    if (!this.stamps[value]) {
      this.stamps[value] = new Stamp(value)
    }
    return this.stamps[value]
  }
}
function main() {
  const factory = new StampFactory()
  const stamps = [
    factory.get('a'),
    factory.get('p'),
    factory.get('p'),
    factory.get('l'),
    factory.get('e'),
  ]
  for (const stamp of stamps) {
    stamp.print()
  }
}
```

Proxy

- 作業に時間がかかっているオブジェクトの代わりに

```typescript
interface Image {
  render()
}
class RealImage implements Image {
  constructor(path: string) { /* 画像読み込み処理 */ }
  render() { /* 画像を表示 */ }
}
class LazyLoadImage implements Image {
  constructor(path: string) { this.path = path }
  render() {
    const realImage = new RealImage(path)
    realImage.render()
  }
}
function main() {
  const image1 = new LazyLoadImage('image1.png')
  const image2 = new LazyLoadImage('image2.png')
  const image3 = new LazyLoadImage('image3.png')

  image1.render() // 画像が読み込まれ表示される
  image2.render() // 画像が読み込まれ表示される
  // image3の不要な読み込み処理は行われない
}
```

Chain of Responsibility

- 「責任者」を「鎖状」につないでおき、「いずれかの段階」で、「誰か」が処理をすることを表現するためのパターン

```typescript
abstract class Responsible {
  setNext(next: Responsible) { this.next = next }
  salaryRaise(score: int): boolean {
    if (this.isMoreThanExpected(store)) {
      if (this.next) {
        return this.next.salaryRaise()
      } else {
        return true
      }
    } else {
      return false
    }
  }
  abstract isMoreThanExpected(store: int): boolean
}
class Staff extends Responsible {
  isMoreThanExpected(score: int) {
    return score > 50
  }
}
class TeamLeader extends Responsible {
  isMoreThanExpected(score: int) {
    return score > 70
  }
}
class Manager extends Responsible {
  isMoreThanExpected(score: int) {
    return score > 90
  }
}
function main() {
  const staff = new Staff()
  const teamLeader = new TeamLeader()
  const manager = new Manager()

  staff.setNext(teamLeader).setNext(manager)
  staff.salaryRaise(80) // false
  staff.salaryRaise(95) // true
}
```

Command

- 要求をCommandオブジェクトにして、それらを複数組み合わせて使えるようにするパターン

```typescript
interface Command {
  execute()
}
class SaladCommand implements Command {
  execute() { /* サラダを調理 >> 提供 */ }
}
class CurryCommand implements Command {
  execute() { /* カレーを調理 >> 提供 */ }
}
class SoupCommand implements Command {
  execute() { /* スープを調理 >> 提供 */ }
}
class Order {
  constructor() {
    this.commands = []
  }
  add(command: Command) {
    this.commands.push(command)
  }
  confirm() {
    for (const command of this.commands) {
      command.execute()
    }
  }
}
function main() {
  const order = new Order()
  order.add(new SaladCommand())
  order.add(new CurryCommand())
  order.add(new SoupCommand())
  order.confirm()
}
```

Interpreter

- TBD

Iterator

- 要素の集まりを保有するオブジェクトの各要素に順番にアクセスする方法を提供するためのパターン

```typescript
interface Iterator {
  hasNext(): boolean
  next(): any
}
interface Aggregate {
  iterator(): Iterator
}
class UserListIterator implements Iterator {
  index: 0
  constructor(userList: UserList) { this.userList = userList }
  hasNext(): boolean { return (index < users.getSize()) }
  next(): any { return this.getAt[index++] }
}
class UserList implements Aggregate {
  users: []
  add(user: string) { this.users.push(user) }
  getSize(): int { return this.users.length }
  getAt(index): string { this.users[index] }
  iterator(): Iterator { return new UserListIterator(this) }
}
function main() {
  const userList = new UserList()
  userList.add ('田中')
  userList.add('佐藤')
  userList.add('山田')

  const iterator = userList.iterator()
  while (iterator.hasNext()) {
    console.log(iterator.next())
  }
}
```

Mediator

- 複数のオブジェクト間の調整をするために、仲介役のクラスを利用するパターン

```typescript
interface ChatRoomMediator {
  showMessage(user: User, message: string)
}
class ChatRoom implements ChatRoomMediator {
  showMessage(user: User, message: string) {
    console.log(user.name, message)
  }
}
class User {
  constructor(name: string, mediator: Mediator) {
    this.name = name
    this.mediator = mediator
  }
  send(message: string) {
    this.mediator.showMessage(this, message)
  }
}
function main() {
  const mediator = new ChatRoom()

  const tom = new User('Tom', mediator)
  const bob = new User('Bob', mediator)

  tom.send('Hello')
  bob.send('Hi')
}
```

Memento

- インスタンスの状態を保存し、後から状態を復元するためのパターン

```typescript
class EditorMemento {
  constructor(content: string) { this.content = content }
  getContent() { return this.content }
}
class Editor {
  content = ''
  type(text: string) { this.content += text }
  getContent(): string { return this.content }
  save(): EditorMemento { return new EditorMemento(this.content) }
  restore(mement: EditorMemento) { this.content = mement.getContent() }
}
function main() {
  const editor = new Editor()

  editor.type('abc')
  editor.type('def')
  const saved = editor.save()

  editor.type('ghi')
  editor.getContent() // abcdefghi

  editor.restore(saved)
  editor.getContent() // abcdef
}
```

Observer

- 通知の仕組みをより汎用的に利用できる形で提供するためのパターン

```typescript
interface Observer {
  update(jobPost: string)
}
class JobSeeker implements Observer {
  update(jobPost: string) {
    console.log(`${jobPost}の求人が追加されました`)
  }
}
interface Subject {
  notify(jobPost: string)
}
class JobPostings implements Subject {
  attach(jobSeeker: JobSeeker) { this.jobSeekers.push(jobSeeker) }
  addJob(jobPost: string) { this.notify(jobPost) }
  notify(jobPost: string) {
    for (const jobSeeker of this.jobSeekers) {
      jobSeeker.update(jobPost)
    }
  }
}
function main() {
  const jobSeeker = new JobSeeker()
  const jobPostings = new JobPostings()
  jobPostings.attach(jobSeeker)

  jobPostings.addJob('エンジニア') // エンジニアの求人が追加されました
  jobPostings.addJob('デザイナー') // デザイナーの求人が追加されました
}
```

State

- 状態に応じて振る舞いを変化させるためのパターン

```typescript
interface State {
  write(text: string)
}
class UpperCaseState implements State {
  write(text: string) { console.log(text.toUpperCase()) }
}
class LowerCaseState implements State {
  write(text: string) { console.log(text.toLowerCase()) }
}
class TextEditor {
  setState(state: State) { this.state = state }
  type(text: string) { this.state.write(text) }
}
function main() {
  const editor = new TextEditor()

  editor.setState(new UpperCaseState())
  editor.type('Hello World') // HELLO WORLD

  editor.setState(new LowerCaseState())
  editor.type('Hello World') // hello world
}
```

Strategy

- アルゴリズムやロジックの実装を切り替えるためのパターン

```typescript
interface SortStrategy {
  sort(array: [])
}
class BubbleSortStrategy implements SortStrategy {
  sort(array: []) { /* バブルソート処理 */ }
}
class QuickSortStrategy implements SortStrategy {
  sort(array: []) { /* クイックソート処理 */ }
}
class Sorter {
  constructor(strategy: SortStrategy) { this.strategy = strategy }
  sort(array: []) { this.strategy.sort(array) }
}
function main() {
  const array = [1, 5, 3, 2]

  const sorter1 = new Sorter(new BubbleSortStrategy())
  sorter1.sort(array)

  const sorter2 = new Sorter(new QuickSortStrategy())
  sorter2.sort(array)
}
```

Template Method

- スーパークラスで処理の枠組みを定め、サブクラスでその具体的内容を実装するパターン

```typescript
abstract class AppBuilder {
  build() {
    this.test()
    this.assemble()
    this.deploy()
  }
  abstract test()
  abstract assemble()
  abstract deploy()
}
class AndroidBuilder extends AppBuilder {
  test() { /* Androidテスト処理 */ }
  assemble() { /* Androidアセンブル処理 */ }
  deploy() { /* Androidデプロイ処理 */ }
}
class IosBuilder extends AppBuilder {
  test() { /* iOSテスト処理 */ }
  assemble() { /* iOSアセンブル処理 */ }
  deploy() { /* iOSデプロイ処理 */ }
}
function main() {
  const androidBuilder = new AndroidBuilder()
  androidBuilder.build()

  const iosBuilder = new IosBuilder()
  iosBuilder.build()
}
```

Visitor

- 受け入れる側に処理を追加することなく、処理を追加することができるパターン

```typescript
interface AnimalOperation {
  visitMonkey(monkey: Monkey)
  visitLion(lion: Lion)
}
interface Animal {
  accept(operation: AnimalOperation)
}
class Monkey implements Animal {
  shout() { console.log('ウキャッ、キャッ') }
  accept(operation: AnimalOperation) {
    operation.visitMonkey(this)
  }
}
class Lion implements Animal {
  roar() { console.log('ガオォオオ') }
  accept(operation: AnimalOperation) {
    operation.visitLion(this)
  }
}
class BarkOperation implements AnimalOperation {
  visitMonkey(monkey: Monkey) {
    monkey.shout()
  }
  visitLion(lion: Lion) {
    lion.roar()
  }
}
function main() {
  const bark = new BarkOperation()

  const monkey = new Monkey()
  monkey.accept(bark)

  const lion = new Lion()
  lion.accept(bark)
}
```

SOLID原則

- Single responsibility principle
  - あるモジュールやクラスや関数などを改修する理由はたった1つになるようにする
  - 1つのクラスが複数の役割を持っている→変更時に他の役割にも影響を与えてしまう
- Open-closed principle
  - 拡張に対してオープンで、修正に対してクローズドにする
  - 機能を追加する時に既存コードの修正が必要→既存の振る舞いが変わってしまう
- Liskov substitution principle
  - 子クラスは親クラスと置き換え可能にする
  - 置換可能でない→抽象以外に依存している→密結合になってしまう
- Interface segregation principle
  - インターフェースを複雑にしてはいけないので、分離できるものは分離する
  - インターフェースが複雑→複数の役割を持っている→変更時に受ける影響が大きくなる
- Dependency inversion principles
  - 依存方向を逆転させることで、設計上望ましい依存の方向性にする
  - 望ましい依存の仕方→双方向依存しない・依存関係が循環しない・必要以上の情報を受け取らず疎結合

UML（Unified Modeling Language）

- ソフトウェアの機能や構造を表す「図」の描き方
- モデリング
  - 言葉で表現しきれない抽象的・複雑・あいまいな対象を、目的に合わないもの除きシンプルにした上で、「模型」や「図」など視覚的に表現すること
- ダイアグラム
  - 構造
    - クラス図
      - クラス間の関係や、各クラスの変数などを表現したもの
    - 複合構造図
    - コンポーネント図
    - オブジェクト図
      - インスタンス間の関係を表現したもの
    - パッケージ図
      - パッケージ間の関係を表現したもの
  - 振る舞い
    - ユースケース図
      - システムが提供する機能と利用者の関係を表現したもの
    - アクティビティ図
      - 一連の処理における制御の流れを表現したもの
    - ステートマシン図
  - 相互作用
    - シーケンス図
      - オブジェクト間の連携や動作の流れを表現したもの
    - コミュニケーション図
      - インスタンス間の相互作用を構造を中心に表現したもの
    - 相互作用概要図
    - タイミング図


# 参考資料
- [デザインパターン | TECHSCORE(テックスコア)](https://www.techscore.com/tech/DesignPattern/index.html/)
- [GoFのデザインパターンまとめ - Qiita](https://qiita.com/i-tanaka730/items/c63c6c22abd1477e0ba0)
- [[保存版]人間が読んで理解できるデザインパターン解説#3: 振舞い系（翻訳）｜TechRacho（テックラッチョ）〜エンジニアの「？」を「！」に〜｜BPS株式会社](https://techracho.bpsinc.jp/hachi8833/2017_10_17/46071)
- [よくわかるSOLID原則1: S（単一責任の原則）｜erukiti｜note](https://note.com/erukiti/n/n67b323d1f7c5)
- [よくわかるSOLID原則2: O（オープン・クローズドの原則）｜erukiti｜note](https://note.com/erukiti/n/n959277a74dd0)
- [よくわかるSOLID原則3: L（リスコフの置換原則）｜erukiti｜note](https://note.com/erukiti/n/n88b8ed99f1e1)
- [よくわかるSOLID原則4: I（インターフェース分離の原則）｜erukiti｜note](https://note.com/erukiti/n/n3daa55541bc8)
- [UML | TECHSCORE(テックスコア)](https://www.techscore.com/tech/UML/index.html/)
- [よく聞くUMLって何？ - Qiita](https://qiita.com/github129/items/80d39f2f043489033076)
