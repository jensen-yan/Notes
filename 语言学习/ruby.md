

```ruby
# hash
books["The deep end"]  = :abysmal	# 使用symbol 冒号开头
books["Living colors"] = :mediocre

puts books
# {"Gravitys Rainbow"=>"splendid", "The deep end"=>"abysmal", "Living colors"=>"mediocre"}

puts books.length

ratings = Hash.new {0}

books.values.each { |rate|
  ratings[rate] += 1  
}

puts books.values
# ["splendid", "abysmal", "mediocre", "mediocre"]
puts ratings
# {"splendid"=>1, "abysmal"=>1, "mediocre"=>2}

# for 循环
5.times { |time|
  puts time  
}
# 1 2 3 4 5


# 对于函数的参数， 可以省略括号！
puts "Hello"
puts("Hello")
poem.gsub(“toast”, “honeydew”)
poem.gsub "toast", "honeydew"


# 自己定义函数
def tame(number_of_shrews)
  number_of_shrews.times {
    puts "Tames"
  }
  return number_of_shrews # 返回值
end


def count_plays(year)
  s = get_shakey
  
  s["William Shakespeare"]
  .select{ |k, v|
    v["finished"] == year	# 筛选
    }.each{ |key, val|
      puts val["title"]
    }.count		# 返回值
  
end

puts count_plays(1591)

# 用 #{} 进行字符串替换
name = "yanyue"
puts "hi, my name is #{name}"
puts "#{v["title"].ljust(30)} #{v["finished"]}"  # ljust（30） 30列对齐

if 1 < 2
  puts "1 < 2"
end

puts "1 < 2" if 1 < 2  # 等价的

# 数据类型
true, false, nil

if ...
  true
else ...
   false
end 	# 后面必须有end！


# 类
class Blurb
  attr_accessor :count, :time, :mood
    # 成员变量
  def initialize(mood, content="")
    @time		= Time
    @content	= content[0..39]
    @mood		= mood
  end
end

blurb1 = Blurb.new
blurb1.content = "todya is asd"
blurb1.time = Time.now
blurb1.mood = :sick

Blurb2 = Blurb.new :confused, "I can sd"

class Blurbalizer
  def initialize(title)
    @title  = title
    @blurbs = [] # A fresh clean array
                 # for storing Blurbs
  end

  def add_a_blurb(mood, content)
    # The << means add to the end of the array
      # 用 << 来添加到数组中
    @blurbs << Blurb.new(mood, content)
  end

  def show_timeline
    puts "Blurbify: #{@title} has #{@blurbs.count} Blurbs"

    @blurbs.sort_by { |t|
      t.time
    }.reverse.each { |t|
      puts "#{t.content.ljust(40)} #{t.time}"
    }
  end
end

myapp = Blurbalizer.new "The Big Blurb"
```

