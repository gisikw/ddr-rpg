require 'tempfile'

# 300 DPI = 825x1275 images
PAGE_SIZE = '825x1275'

# TODO: Check bins exist

task :build_pages do
  `curl -s https://via.placeholder.com/825x1275.png?text=page%201 > ./pages/1.png`
  `curl -s https://via.placeholder.com/825x1275.png?text=page%202 > ./pages/2.png`
  `curl -s https://via.placeholder.com/825x1275.png?text=page%203 > ./pages/3.png`
  `curl -s https://via.placeholder.com/825x1275.png?text=page%204 > ./pages/4.png`
  `curl -s https://via.placeholder.com/825x1275.png?text=page%205 > ./pages/5.png`
  `curl -s https://via.placeholder.com/825x1275.png?text=page%206 > ./pages/6.png`
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
