[[vim Makefile]]

``` Makefile
# 기본적으로 실행되는 타겟
first:
    # config.mk 파일이 없다면, src/config.mk.dist 파일을 복사하여 기본 구성 파일로 설정
	@if test ! -f src/auto/config.mk; then \
		cp src/config.mk.dist src/auto/config.mk; \
	fi
	@echo "Starting make in the src directory."
	@echo "If there are problems, cd to the src directory and run make there"
	# src 디렉터리에서 make를 실행
	# vim 의 실제 소스코드가 있는 src 디렉터리로 이동한 뒤, 거기서 make 실행함 
	cd src && $(MAKE) $@

# all, install, clean 등 일반적인 타겟이 한 줄로 나열되어 있음
# make install 등 을 실행하면 이 타겟에서 정의된 명령어를 실행하게 됨
all install uninstall tools config configure reconfig proto depend lint tags types test scripttests testtiny test_libvterm unittests testclean clean distclean:
    # config.mk 파일이 없을떄, src/config.mk.dist 복사
	@if test ! -f src/auto/config.mk; then \
		cp src/config.mk.dist src/auto/config.mk; \
	fi
	@echo "Starting make in the src directory."
	@echo "If there are problems, cd to the src directory and run make there"
	# src 디렉터리로 이동 후 make
	cd src && $(MAKE) $@
	@# When the target is "test" also run the indent and syntax tests.
	# indenttext, syntaxtest 진행
	@if test "$@" = "test" -o "$@" = "testtiny"; then \
		$(MAKE) indenttest; \
		$(MAKE) syntaxtest; \
	fi
	@# When the target is "clean" also clean for the indent and syntax tests.
	@if test "$@" = "clean" -o "$@" = "distclean" -o "$@" = "testclean"; then \
		(cd runtime/indent && $(MAKE) clean); \
		(cd runtime/syntax && $(MAKE) clean); \
	fi
```

**`indenttest`**

``` Makefile
# Executable used for running the indent tests.
VIM_FOR_INDENTTEST = ../../src/vim

# runtime/syntax 폴더로 이동 후, clean test
# VIMPROG 변수로 설정된 Vim 실행 파일을 사용해 테스트
indenttest:
	cd runtime/indent && \
		$(MAKE) clean && \
		$(MAKE) test VIMPROG="$(VIM_FOR_INDENTTEST)"
```

**`syntaxtest`**

``` Makefile
# Executable used for running the syntax tests.
VIM_FOR_SYNTAXTEST = ../../src/vim

# (For local testing only with GNU Make.)
VIM_SYNTAX_TEST_FILTER =

# vim의 구문 강조 테스트를 수행하는 타겟
# runtime/syntax 디렉터리로 이동해서 clean 및 test 명령어를 실행
# VIMPROG를 이용해서 Vim 실행 후 test 수행
syntaxtest:
	cd runtime/syntax && \
		$(MAKE) clean && \
		$(MAKE) test VIMPROG="$(VIM_FOR_SYNTAXTEST)"
```

``unixall``

```Makefile
# Unix 환경에서 Vim 소스코드와 실행 파일 패키징
unixall: dist prepare
    # 이전 빌드 결과 정리
	-rm -f dist/$(VIMVER).tar.bz2
	-rm -rf dist/$(VIMRTDIR)
	# 새로운 디렉터리 생성
	mkdir dist/$(VIMRTDIR)
	# 압축
	tar cf - \
		$(RT_ALL) \
		$(RT_ALL_BIN) \
		$(RT_UNIX) \
		$(RT_UNIX_DOS_BIN) \
		$(RT_SCRIPTS) \
		$(LANG_GEN) \
		$(LANG_GEN_BIN) \
		$(SRC_ALL) \
		$(SRC_UNIX) \
		$(SRC_DOS_UNIX) \
		$(EXTRA) \
		$(LANG_SRC) \
		| (cd dist/$(VIMRTDIR); tar xf -)
	# readme 삭제
	-rm $(IN_README_DIR)
# Need to use a "distclean" config.mk file
# Note: this file is not included in the repository to avoid problems, but it's
# OK to put it in the archive.
    # 설정 파일 복사
	cp -f src/config.mk.dist dist/$(VIMRTDIR)/src/auto/config.mk
# Create an empty config.h file, make dependencies require it
    touch 명령어로 타임스탬프 조정
	touch dist/$(VIMRTDIR)/src/auto/config.h
# Make sure configure is newer than config.mk to force it to be generated
	touch dist/$(VIMRTDIR)/src/configure
# Make sure ja.sjis.po is newer than ja.po to avoid it being regenerated.
# Same for cs.cp1250.po, pl.cp1250.po and sk.cp1250.po.
	touch dist/$(VIMRTDIR)/src/po/ja.sjis.po
	touch dist/$(VIMRTDIR)/src/po/cs.cp1250.po
	touch dist/$(VIMRTDIR)/src/po/pl.cp1250.po
	touch dist/$(VIMRTDIR)/src/po/sk.cp1250.po
	touch dist/$(VIMRTDIR)/src/po/zh_CN.cp936.po
	touch dist/$(VIMRTDIR)/src/po/ru.cp1251.po
	touch dist/$(VIMRTDIR)/src/po/uk.cp1251.po
# Create the archive.
    # tar 파일 생성 및 압축
	cd dist && tar cf $(VIMVER).tar $(VIMRTDIR)
	bzip2 dist/$(VIMVER).tar
```