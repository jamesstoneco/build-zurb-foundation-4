# Build Zurb Foundation 4
# 
# A simple Rake build script to get your Sass / Compass Zurb Foundation 4 Projects ready for production
#
# Created by James Stone
# manofstone.com
#
# source and repo
# github.com/manofstone/build-foundation
#
# MIT License
# Go Crazy, Stay Fired Up!
#
# Requires:
#
# UglifierJS
# npm install uglify-js
# 
# cleancss
# npm install clean-css
#
# Usage:
# rake
# rake build

JS_DIR              = 'javascripts'
JS_CAT_FILENAME     = 'app.js'
CSS_DIR             = 'stylesheets'
CSS_FILENAME        = 'app.css'
BUILD_DIR           = 'build'
IMAGE_DIR           = 'images'
UGLIFIER_OPTS       = '--verbose'
HTML_FILES          = ['index.html']
JS_INDIVIDUAL_FILES = ["vendor/custom.modernizr.js", "vendor/jquery.js", "vendor/zepto.js"]
JS_FILES_TO_CONCAT  = ["foundation/foundation.js", "foundation/foundation.abide.js", "foundation/foundation.alerts.js", "foundation/foundation.clearing.js", "foundation/foundation.cookie.js", "foundation/foundation.dropdown.js", "foundation/foundation.forms.js", "foundation/foundation.interchange.js", "foundation/foundation.joyride.js", "foundation/foundation.magellan.js", "foundation/foundation.orbit.js", "foundation/foundation.placeholder.js", "foundation/foundation.reveal.js", "foundation/foundation.section.js", "foundation/foundation.tooltips.js", "foundation/foundation.topbar.js"]

def find_replace_in_file (the_file, search_term, replace_term)
  File.open(the_file) { |source_file|
    contents = source_file.read
    contents.gsub!(search_term, replace_term)
    File.open(the_file, "w+") { |f| f.write(contents) }
  }
end

task :default => [:build]

task :build do
  sh "rm -r #{BUILD_DIR}"
  puts "creating #{BUILD_DIR}/ directory"
  sh "mkdir #{BUILD_DIR}"
  sh "mkdir #{BUILD_DIR}/#{JS_DIR}"
  sh "mkdir #{BUILD_DIR}/#{JS_DIR}/vendor"
  sh "mkdir #{BUILD_DIR}/#{CSS_DIR}"
  sh "mkdir #{BUILD_DIR}/images"

  puts 'minifying seperate js files'
  JS_INDIVIDUAL_FILES.each do | j |
    sh "cp #{JS_DIR}/#{j} #{BUILD_DIR}/#{JS_DIR}/#{j}"
    sh "uglifyjs #{UGLIFIER_OPTS} #{BUILD_DIR}/#{JS_DIR}/#{j} -mo #{BUILD_DIR}/#{JS_DIR}/#{j}.min"
  end

  puts 'minifying combined js'

  JS_FILES_TO_CONCAT.map { | file | "/#{JS_DIR}/#{file}" }
  files_to_cat = JS_FILES_TO_CONCAT.map { | file | "#{JS_DIR}/#{file}"}
  system "cat #{files_to_cat.join(' ')} > #{BUILD_DIR}/#{JS_DIR}/#{JS_CAT_FILENAME}"
  sh "uglifyjs #{UGLIFIER_OPTS} #{BUILD_DIR}/#{JS_DIR}/#{JS_CAT_FILENAME} -mo #{BUILD_DIR}/#{JS_DIR}/#{JS_CAT_FILENAME}.min"

  puts 'compressing css'
  sh 'compass compile -e production --force'
  sh "cp #{CSS_DIR}/#{CSS_FILENAME} #{BUILD_DIR}/#{CSS_DIR}/#{CSS_FILENAME}"
  sh "cleancss -e -o #{BUILD_DIR}/#{CSS_DIR}/#{CSS_FILENAME}.min #{BUILD_DIR}/#{CSS_DIR}/#{CSS_FILENAME}"

  puts 'copying media files'
  system "cp -r #{IMAGE_DIR} #{BUILD_DIR}/#{IMAGE_DIR}"

  puts 'copying over html'
  sh "cp *.html #{BUILD_DIR}"

  puts 'modify html to use minified and concated resources'
  HTML_FILES.each do | h |
    puts h
    # Revise CSS ref.
    find_replace_in_file "#{BUILD_DIR}/#{h}", "<link rel=\"stylesheet\" href=\"#{CSS_DIR}/#{CSS_FILENAME}\">", "<link rel=\"stylesheet\" href=\"#{CSS_DIR}/#{CSS_FILENAME}.min\">"
    # Revise jQuery Zepto Proto ref.
    find_replace_in_file "#{BUILD_DIR}/#{h}", "'.js><\\/script>'", "'.js.min><\\/script>'"
    JS_FILES_TO_CONCAT.each_with_index do | j, i |
      puts "  removing ref to #{j}"
      # Replace first found to JS_CAT_FILENAME otherwise remove ref.
      found_str = "<script src=\"#{JS_DIR}/#{j}\"></script>"
      if i == 0
        find_replace_in_file "#{BUILD_DIR}/#{h}", found_str, "<script src=\"#{JS_DIR}/#{JS_CAT_FILENAME}\"></script>"
      else
        find_replace_in_file "#{BUILD_DIR}/#{h}", found_str, ''
      end
    end
  end
end