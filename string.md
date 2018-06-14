# Stringクラス
## クラスメソッド
---
---
### new
```ruby
String.new(str = "")
```
クラスメソッドnewは、新しい文字列（Stringクラスのインスタンス）を作成して返します。引数を省略したときは、空文字列を作成します。引数に文字列を指定したときは、その文字列のコピーを返します。

```ruby
s = String.new
p s #=>""
s = String.new("Hello")
p s #=>"Hello"
```
---
### try_convert
```ruby
String.try_convert(obj)
```
クラスメソッドtry_convertは、引数objが持っているto_strメソッドを呼び出し、文字列を返します。objにto_strメソッドがなければnilを返します。
objのto_strメソッドが文字列以外のオブジェクトを返したときは、例外TypeErrorが発生します。

```ruby
p String.try_convert("hello") #=>"hello
p String.try_convert(123) #=>nil
```
---
## インスタンスメソッド
---
---
### %
```ruby
str % arg
```
%演算子（メソッド）は、書式付き文字列を作成します。左辺の文字列strで表した書式に右辺のargを埋め込んで、新しい文字列を返します。sprintf(str, arg)と同じです。
```ruby
num = 10.0 / 3
puts "%.4f" % num # =>3.3333
```
書式に埋め込む値が複数ある場合は、右辺を配列にします。
```ruby
total = 300
average = 123.456
puts "total %d, average %.1f" % [total, average] 
#=>total 300, average 123.5
```
---
### *
```ruby
str * integer
```
*演算子（メソッド）は、左辺の文字列strを右辺の数integerだけ繰り返し並べた文字列を返します。
```ruby
str = "Hello"
puts str * 3 #=>HelloHelloHello
```
---
### +
```ruby
str + other_str
```
+メソッド（演算子）は、2つの文字列を連結した新しい文字列を返します。
```ruby
s = "Hello"
puts s + ", world" #=>Hello, world
```
---
### <<
```ruby
str << other_str
```
<<演算子（メソッド）は、文字列strの末尾に別の文字列other_strを加えます。+とは違い、レシーバ自身の文字列を変更します。戻り値はレシーバ自身です。
concatメソッドは<<の別名です。
```ruby
s = "Hello"
s << ", world"
puts s #=>Hello, world
```
```ruby
str <<  integer
```
文字列の代わりに整数で文字のコードを指定すると、文字列の末尾に1文字追加します。指定できる整数は0から255です。
```ruby
s = "Hello, world"
s << 0x21
puts s #=>Hello, world!
```
Ruby 1.9では256以上の整数を指定できます。Shift_JISの文字列に2バイトの文字を追加したり、UTF-8の文字列にユニコード番号で文字を追加したりできます。

---
### ==
```ruby
str == other_str
```
==演算子（メソッド）は、2つの文字列が同じかどうかを調べます。文字列の内容が同じならtrue、違いがあればfalseを返します。

右辺other_strが文字列でない場合はfalseを返します。"123" == 123はfalseです。

2つの文字列が別のStringオブジェクトであっても、持っている文字列が同じならtrueになります。「同じオブジェクトかどうか」を調べたいときは、Objectクラスのequal?メソッドを使います。
```ruby
s = "hello"
t = "hello"
puts s == t #=>true
puts s.equal?(t) #=>false
```
!=演算子は、2つの文字列が違えばtrue、同じならfalseを返します。!=はStringクラスで定義されているメソッドではありませんが、Rubyでは==が定義されていれば自動的に!=が使えます。
```ruby
s = "hello"
t = "world"
puts s == t #=>false
puts s != t #=>true
```
#### 詳細
Ruby 1.9では、文字列の文字コード（Encodingオブジェクト）が違えば、同じ文字を持っていてもfalseになります。ただし、文字がすべてアスキー文字の範囲ならtrueになります。
```ruby
# encoding: utf-8
s = "太郎"
t = "太郎".encode("Shift_JIS")
puts s == t #=>false

s = "hello"
t = "hello".encode("Shift_JIS")
puts s == t #=>true
```
組み込み変数$=の値がtrueであれば、==は大文字小文字を区別しないで比較します。ただし、$=はRuby 1.8で廃止予定になり、Ruby 1.9で廃止されました。大文字小文字を区別しないで比較するには、casecmpメソッドを使います。

---
### <, <=, >, >=
```ruby
str < other_str
str <= other_str
str > other_str
str >= other_str
```
<、<=、>、>=の各演算子（メソッド）は、2つの文字列の大小、つまり辞書的な順番を調べます。それぞれ、左辺が先、左辺が先または同じ、右辺が先、右辺が先または同じ、であればtrueが返り、そうでなければfalseが返ります。右辺other_strがStringオブジェクトでないときは例外ArgumentErrorが発生します。
```ruby
word1 = "happily"
word2 = "happines"
word3 = "happlily"
puts word1 < word2 #=>true
puts word1 > word2 #=>false
puts word1 <= word3 #=>true
puts word1 >= word2 #=>false
```
#### 詳細
<、<=、>、>=は、Stringクラスで定義されているものではなく、Comparableモジュールから取り込まれたものです。

文字列の順序は、バイト列の単純な比較によります。"α" < "あ"は、文字コードがUTF-8なら結果はtrue、Shift_JISならfalseになります。

---
### <=>
```ruby
str <=> other_str
```
<=>演算子（メソッド）は、2つの文字列の辞書的な順番を調べます。左辺が先なら-1、同じなら0、右辺が先なら1を返します。右辺がStringオブジェクトでないときはnilが返ります。
```ruby
word1 = "happily"
word2 = "happines"
puts word1 <=> word2 #=>-1
```
文字列の順序は、バイト列の単純な比較によります。"α" <=> "あ"は、文字コードがUTF-8なら結果は-1、Shift_JISなら1になります。

---
### =~
```ruby
str =~ regexp
```
=~演算子（メソッド）は、文字列strに対して正規表現regexpとのパターンマッチを行います。マッチしたときは、マッチした部分の位置を整数で返します。マッチしなかったら、nilを返します。

右辺のregexpにRegexp以外のオブジェクトを指定するとfalseが返ります。ただし、右辺に文字列を指定したときは、例外TypeErrorが発生します。

=~でマッチに成功したときは、組み込み変数$&、$1、$2などに値がセットされます。次の例では、文字列とパターン/<(.+)>/をマッチさせ、< >に囲われた語句を変数$1で取り出しています。
```ruby
str = "hello <ruby> world"
if str =~ /<(¥w+)>/
  puts $1 #=>ruby
end
```
!~演算子は、マッチしなかったときにtrueを、マッチしたときにfalseを返します。!~はStringクラスで定義されているメソッドではありませんが、Rubyでは=~が定義されていれば自動的に!~が使えます。
```ruby
str = "hello <ruby> world"
if str !~ /¥d/
  puts "digit not found" #=>digit not found
end
```
---
### []
[]は、文字列の中から部分文字列を取り出すメソッドです。s[2]、s[3,5]、s[2..7]、s[/[0-9]/] のように、いろいろな形で利用できます。配列要素の取り出しのように記述しますが、実際にはメソッド呼び出しです。[]の中はメソッドの引数です。

sliceメソッドは、[]の別名です。
#### 位置
```ruby
str[idx]
```
Ruby 1.9では、引数に整数を1つ指定すると、その位置の文字を返します。バイトごとではなく文字ごとの位置です。バイトのコードを得たい場合はgetbyteメソッドを使います。
```ruby
# encoding: utf-8
s = "hello"
puts s[1] #=>e
puts s[-2] #=>l
s = "こんにちは"
puts s[1] #=>ん
```
#### 位置と数
```ruby
str[idx, Len]
```
Ruby 1.9では、位置と数はバイトごとではなく文字ごとになります。
```ruby
# encoding: utf-8
s = "こんにちは"
puts s[2, 2] #=>にち
```
#### 範囲
```ruby
str[range]
```
引数に範囲を指定すると、その範囲に対応する部分文字列を返します。範囲外の位置を指定すると、nilが返ります。
```ruby
s = "hello, world"
puts s[7..10] #=>worl
puts s[7...10] #=>wor
```
開始位置と終了位置がマイナスの場合は、文字列の末尾から数えます（-1が末尾から1番目、-2が末尾から2番目、...）。
```ruby
s = "hello, world"
puts s[-5..-1]  #=>world
```
str[idx, len] の場合と同様に、Ruby 1.8では範囲はバイトごとに数えられます。Ruby 1.9では範囲は文字ごとに数えられます。
