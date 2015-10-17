require 'yaml'

class Chapter
  attr_accessor :id, :title, :number
  attr_reader :filename

  def initialize(options={})
    options.each do |key, value|
      instance_variable_set(key.to_s.prepend('@').intern, value)
    end

    yield(self) if block_given?
  end

  def create!
    File.open(filename, 'w') do |f|
      f.puts YAML.dump('title' => title, 'permalink' => "chapter/#{number}.html", 'layout' => 'chapter')
      f.puts "---"
      f.puts '<a name="{{ page.permalink }}"></a> <!-- Leave this here -->'
    end
  end

  def last_chapter
    @last_chapter ||= Chapter.new do |c|
      filename = Dir.glob("_posts/*").sort.last
      c.id = filename.match(/(\d{4}-\d{2}-\d{2})/)[0]
      front_matter = YAML.load(File.read(filename).match(/---((.|\n)*)---/)[1])
      c.title = front_matter['title']
      c.number = front_matter['permalink'].match(/chapter\/(\d).html/)[1]
    end
  end

  def has_previous_chapter?
    !Dir.glob("_posts/").empty?
  end

  def filename
    "_posts/#{id}-#{tokenize(title)}.md"
  end
private
  def tokenize(str)
    str.gsub(/\W/, '-').gsub(/\W\z/, '').downcase
  end
end

namespace :generate do
  desc "Generate Chapter"
  task :chapter, [:title] do |t, args|
    @title = args[:title] if args[:title]

    chapter = Chapter.new do |c|
      c.id =
        if c.has_previous_chapter?
          c.last_chapter.id.succ
        else
          "1900-01-01"
        end

      c.number =
        if @number
          @number
        elsif c.has_previous_chapter?
          c.last_chapter.number.succ
        else
          '1'
        end

      c.title = @title || "Chapter #{c.number}"
    end

    chapter.create!
    puts "Created new chapter \"#{chapter.title}\" at #{chapter.filename}"
  end
end
