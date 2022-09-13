require 'tempfile'

# 300 DPI = 825x1275 images
WIDTH=825
HEIGHT=1275
PAGE_SIZE = "#{WIDTH}x#{HEIGHT}"
CUSTOM_WK_HTML_CONFIG = "--width #{WIDTH} --height #{HEIGHT} --user-style-sheet ./src/style.css"

# TODO: Check bins exist: imagemagick, pandoc, wkhtmltoiomage

task :build_pages do
  `curl -s https://via.placeholder.com/825x1275.png?text=cover > ./pages/1.png`
  `curl -s https://via.placeholder.com/825x1275.png?text=character%20sheet > ./pages/2.png`
  `curl -s https://via.placeholder.com/825x1275.png?text=inventory > ./pages/3.png`
  `pandoc ./rules/characters.md | wkhtmltoimage #{CUSTOM_WK_HTML_CONFIG} -  ./pages/4.png 2>/dev/null`
  `pandoc ./rules/archetypes.md | wkhtmltoimage #{CUSTOM_WK_HTML_CONFIG} - ./pages/5.png 2>/dev/null`
  `pandoc ./rules/rules.md | wkhtmltoimage #{CUSTOM_WK_HTML_CONFIG} - ./pages/6.png 2>/dev/null`
  `curl -s https://via.placeholder.com/825x1275.png?text=page%207 > ./pages/7.png`
  `curl -s https://via.placeholder.com/825x1275.png?text=page%208 > ./pages/8.png`
end

task build_pdfs: [:build_pages] do
  `montage \
    ./pages/1.png \
    ./pages/2.png \
    ./pages/3.png \
    ./pages/4.png \
    ./pages/5.png \
    ./pages/6.png \
    ./pages/7.png \
    ./pages/8.png \
    -geometry #{PAGE_SIZE}+0+0 \
    -tile 4x2 \
    ./dist/release.pdf
  `

  # TODO: See if we can consolidate this
  one = Tempfile.new
  seven = Tempfile.new
  six = Tempfile.new
  five = Tempfile.new
  `convert -rotate 180 ./pages/1.png #{one.path}`
  `convert -rotate 180 ./pages/7.png #{seven.path}`
  `convert -rotate 180 ./pages/6.png #{six.path}`
  `convert -rotate 180 ./pages/5.png #{five.path}`
  `montage \
    ./pages/2.png \
    ./pages/3.png \
    ./pages/4.png \
    ./pages/5.png \
    #{one.path} \
    #{seven.path} \
    #{six.path} \
    #{five.path} \
    -geometry #{PAGE_SIZE}+0+0 \
    -tile 4x2 \
    ./dist/booklet.pdf
  `
end

task :default => :build_pdfs
