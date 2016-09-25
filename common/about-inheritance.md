# 繼承是一種 `is-a` 的關係

在很多 OO 的概念文章中應該很常會看到 `is-a` 和 `has-a` 這兩個名詞。

`is-a`，就是 _is a kind of_ 的意思。例如 **Dog is a kind of Animal**。繼承應該要發生在這種關係之下。

`has-a` 指的是組合，例如 **Circus has a Animal**。也就是_整體_與_組件_的關係。

假如今天有人說你的 code 裡面 A class 和 B class 有好多一樣的 methods，你的第一個反應是「**啊～那我寫一個 Class 來讓 A 和 B 繼承就很 OO 了，是不是。DRY, MAN~**」的話，那事情就嚴重了。

這樣其實沒什麼不對，程式也可以正常 work，但把 `is-a` 這個概念從繼承中忽略掉本身卻是一個大問題。少了這個概念很容易寫出讓人看不懂的爛扣，因為本身就缺乏正確的抽象概念，令人難以理解。

例如：

```ruby
class Masarati < Car
  def initialize
    @engine = Engine::V10.new
  end

  def start
    @engine.start
    head_light(:on)
    air_condition(:on)

    puts 'ready to go.'
  end
  #...
end

my_car = Masarati.new
my_car.start # ready to go
```

Masarati **is a** Car, Masarati **has a** V10 engine. 應該很容易理解。

但假如硬要提出 class 作繼承而忽略了 `is-a` 的關係，很可能就會寫出：

```ruby
class User < PokemonApi
  MissingEmail = Class.new(StandardError)
  MissingPassword = Class.new(StandardError)

  def initialize(args = {})
    @email = args.fetch(:email)
    @password = args.fetch(:passowrd)

    raise MissingEmail, 'no email provided' unless @email
    raise MissingPassword, 'no password provided' unless @password
  end

  def login_to_pmgo(location)
    login_by_google_account(email: @email, password: @password)
    set_current_location(location)

    puts @message
  end
  #...
end

my_account = User.new(email: 'a@b.com', password: '12345678')
my_account.login_to_pmgo('國父紀念館') # Getting Probably permabanned, Game Over !
```

很明顯地這個程式可以正常運作，但別人如果突然讀到的時候需要花比較多的時間來理解整個結構。

--------------------------------------------------------------------------------

這個議題和 `Inheritance vs Composition` 相關，什麼時候繼承（`is-a`）、什麼時候組合（`has-a`）在程式設計上其實可以找到很多討論。熟悉 Design Pattern 的人可能會告訴你 Composition over inheritance，想知道為什麼會這樣說的話...可以多看一些[相關的討論](https://www.youtube.com/watch?v=wfMtDGfHWpA)。
