BOOTSTRAP = ../../templates/css/bootstrap.css
BOOTSTRAP_LESS = ./less/bootstrap.less
BOOTSTRAP_RESPONSIVE = ../../templates/css/bootstrap-responsive.css
BOOTSTRAP_RESPONSIVE_LESS = ./less/responsive.less
DATE=$(shell date +%I:%M%p)
CHECK=\033[32m✔\033[39m
HR=\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#


#
# BUILD DOCS
#

build:
	@echo "\n${HR}"
	@echo "Building Bootstrap..."
	@echo "${HR}\n"
	@./node_modules/.bin/jshint js/*.js --config js/.jshintrc
	@./node_modules/.bin/jshint js/tests/unit/*.js --config js/.jshintrc
	@echo "Running JSHint on javascript...             ${CHECK} Done"
	@./node_modules/.bin/recess --compile ${BOOTSTRAP_LESS} > ${BOOTSTRAP}
	@./node_modules/.bin/recess --compile ${BOOTSTRAP_RESPONSIVE_LESS} > ${BOOTSTRAP_RESPONSIVE}
	@echo "Compiling LESS with Recess...               ${CHECK} Done"
	@node docs/build
	@cp img/* ../../templates/img/
	@cp js/*.js ../../templates/js/
	@cp js/tests/vendor/jquery.js ../../templates/js/
	@echo "Compiling documentation...                  ${CHECK} Done"
	@cat js/bootstrap-transition.js js/bootstrap-alert.js js/bootstrap-button.js js/bootstrap-carousel.js js/bootstrap-collapse.js js/bootstrap-dropdown.js js/bootstrap-modal.js js/bootstrap-tooltip.js js/bootstrap-popover.js js/bootstrap-scrollspy.js js/bootstrap-tab.js js/bootstrap-typeahead.js js/bootstrap-affix.js > ../../templates/js/bootstrap.js
	@./node_modules/.bin/uglifyjs -nc docs/assets/js/bootstrap.js > ../../templates/js/bootstrap.min.tmp.js
	@echo "/**\n* Bootstrap.js v2.3.1 by @fat & @mdo\n* Copyright 2012 Twitter, Inc.\n* http://www.apache.org/licenses/LICENSE-2.0.txt\n*/" > docs/assets/js/copyright.js
	@cat docs/assets/js/copyright.js ../../templates/js/bootstrap.min.tmp.js > ../../templates/js/bootstrap.min.js
	@rm docs/assets/js/copyright.js ../../templates/js/bootstrap.min.tmp.js
	@echo "Compiling and minifying javascript...       ${CHECK} Done"
	@echo "\n${HR}"
	@echo "Bootstrap successfully built at ${DATE}."
	@echo "${HR}\n"
	@echo "Thanks for using Bootstrap,"
	@echo "<3 @mdo and @fat\n"

#
# RUN JSHINT & QUNIT TESTS IN PHANTOMJS
#

test:
	./node_modules/.bin/jshint js/*.js --config js/.jshintrc
	./node_modules/.bin/jshint js/tests/unit/*.js --config js/.jshintrc
	node js/tests/server.js &
	phantomjs js/tests/phantom.js "http://localhost:3000/js/tests"
	kill -9 `cat js/tests/pid.txt`
	rm js/tests/pid.txt

#
# CLEANS THE ROOT DIRECTORY OF PRIOR BUILDS
#

clean:
	rm -r bootstrap

#
# BUILD SIMPLE BOOTSTRAP DIRECTORY
# recess & uglifyjs are required
#

bootstrap: bootstrap-img bootstrap-css bootstrap-js


#
# JS COMPILE
#
bootstrap-js: bootstrap/js/*.js

bootstrap/js/*.js: js/*.js
	cat js/bootstrap-transition.js js/bootstrap-alert.js js/bootstrap-button.js js/bootstrap-carousel.js js/bootstrap-collapse.js js/bootstrap-dropdown.js js/bootstrap-modal.js js/bootstrap-tooltip.js js/bootstrap-popover.js js/bootstrap-scrollspy.js js/bootstrap-tab.js js/bootstrap-typeahead.js js/bootstrap-affix.js > ../../templates/js/bootstrap.js
	./node_modules/.bin/uglifyjs -nc ../../templates/js/bootstrap.js > ../../templates/js/bootstrap.min.tmp.js
	echo "/*!\n* Bootstrap.js by @fat & @mdo\n* Copyright 2012 Twitter, Inc.\n* http://www.apache.org/licenses/LICENSE-2.0.txt\n*/" > ../../templates/js/copyright.js
	cat ../../templates/js/copyright.js ../../templates/js/bootstrap.min.tmp.js > ../../templates/js/bootstrap.min.js
	rm ../../templates/js/copyright.js ../../templates/js/bootstrap.min.tmp.js

#
# CSS COMPLILE
#

bootstrap-css: bootstrap/css/*.css

bootstrap/css/*.css: less/*.less
	./node_modules/.bin/recess --compile ${BOOTSTRAP_LESS} > ../../templates/css/bootstrap.css
	./node_modules/.bin/recess --compress ${BOOTSTRAP_LESS} > ../../templates/css/bootstrap.min.css
	./node_modules/.bin/recess --compile ${BOOTSTRAP_RESPONSIVE_LESS} > ../../templates/css/bootstrap-responsive.css
	./node_modules/.bin/recess --compress ${BOOTSTRAP_RESPONSIVE_LESS} > ../../templates/css/bootstrap-responsive.min.css

#
# IMAGES
#

bootstrap-img: bootstrap/img/*

bootstrap/img/*: img/*
	cp img/* ../../templates/img/


#
# MAKE FOR GH-PAGES 4 FAT & MDO ONLY (O_O  )
#

gh-pages: bootstrap docs
	rm -f docs/assets/bootstrap.zip
	zip -r docs/assets/bootstrap.zip bootstrap
	rm -r bootstrap
	rm -f ../bootstrap-gh-pages/assets/bootstrap.zip
	node docs/build production
	cp -r docs/* ../bootstrap-gh-pages

#
# WATCH LESS FILES
#

watch:
	echo "Watching less files..."; \
	watchr -e "watch('less/.*\.less') { system 'make' }"


.PHONY: docs watch gh-pages bootstrap-img bootstrap-css bootstrap-js