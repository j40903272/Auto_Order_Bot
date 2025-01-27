###########################
# General
###########################

.PHONY: test init
test:
	python -m pytest

style-check: flake8-check # black-check

flake8-check:
	python -m flake8

black-check:
	python -m black --line-length 188 --target-version=py37 --check ./

doc:
	python -m pydoc -b


compile:
	pyinstaller --noconsole -D src/gui.py

compile-win:
	# this is for compile windows exe on mac
	mkdir -p build-win
	cp -r src build-win
	cp requirements.txt build-win/src
	cp setup.py build-win/src
	docker run -v "$(shell pwd)/build-win/src:/src/" -v "$(shell pwd)/setup.py:/setup.py" cdrx/pyinstaller-windows "python -m pip install --upgrade pip setuptools && python -m pip install ccxt && export PYTHONPATH=/ && pyinstaller -D gui.py"
	# matplotlib msvc-runtime easyocr numpy pillow
	# telethon pyyaml pysimplegui pandas

compile-linux:
	# this is for compile linux elf on mac
	# never test before
	mkdir -p build-linux
	cp -r src build-linux
	cp requirements.txt build-linux/src
	cp setup.py build-linux/src
	docker run -v "$(shell pwd)/build-linux/src:/src/" -v "$(shell pwd)/setup.py:/setup.py" cdrx/pyinstaller-linux "python -m pip install --upgrade pip setuptools && python -m pip install ccxt && export PYTHONPATH=/ && pyinstaller -D gui.py"


###########################
# Setup environment
###########################

init: create-env install version

create-env: check-env env-dependency install-env

install-env:
	@if ! [ -d $$HOME/.pyenv ]; then \
		curl https://pyenv.run | bash >/dev/null 2>&1 ; \
		if ! grep -Fq "pyenv" $$HOME/.bashrc; then\
			echo "# ===========================" >> $$HOME/.bashrc \
			echo "# Pyenv configuration        " >> $$HOME/.bashrc \
			echo "# ===========================" >> $$HOME/.bashrc \
			echo "export PATH=$$HOME/.pyenv/bin:\$$PATH" >> $$HOME/.bashrc ; \
			echo "eval \"\$$(pyenv init -)\"" >> $$HOME/.bashrc ; \
			echo "eval \"\$$(pyenv virtualenv-init -)\"" >> $$HOME/.bashrc ; \
			echo "# ===========================" >> $$HOME/.bashrc ;\
		fi \
	fi
  # @. $$HOME/.bashrc
  # @if ! (python3 -m pip list --disable-pip-version-check | grep pipenv > /dev/null) ; then \
  #   python3 -m pip install pipenv ; \
  #   if ! grep -Fq "\$$PATH:\$$PYTHON_BIN_PATH" $$HOME/.bashrc; then \
  #     echo "export PATH=\$$PATH:\$$PYTHON_BIN_PATH" >> $$HOME/.bashrc ; \
  #   fi \
  #   if ! grep -Fq "pipenv" $$HOME/.bashrc; then\
  #     echo "export PYTHON_BIN_PATH=$$(python3 -m site --user-base)/bin" >> $$HOME/.bashrc ; \
  #   fi \
  # fi
  # @. $$HOME/.bashrc

uninstall-env:
	python3 -m pip uninstall -y pipenv
	rm -rf $$HOME/.pyenv
	@echo "==========================="
	@echo "Need manual clean up bashrc"
	@echo "==========================="
	vim $$HOME/.bashrc

env-dependency:
	sudo apt update
	sudo apt install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
	libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
	xz-utils tk-dev libffi-dev liblzma-dev python-openssl git libedit-dev

check-env:
	@if ! (python3 -m pip list --disable-pip-version-check | grep pipenv > /dev/null) ; then \
		echo "pipenv not install";\
	fi
	@if ! [ -d $$HOME/.pyenv ]; then\
		echo "pyenv not install";\
	fi
	@if ! grep -Fq "pipenv" $$HOME/.bashrc; then\
		echo "pipenv completion not in bashrc";\
	fi
	@if ! grep -Fq "pyenv" $$HOME/.bashrc; then\
		echo "pyenv not in bashrc";\
	fi
	@if ! grep -Fq "\$$PATH:\$$PYTHON_BIN_PATH" $$HOME/.bashrc; then \
		echo "pipenv PATH not in bashrc";\
	fi \

install:
	pipenv install --dev --skip-lock

uninstall:
	pipenv clean
	pipenv --rm

clean:
	find . -name "*.py[co]" -delete
	find . -name "*~" -delete
	find . -name "__pycache__" -delete

shell:
	pipenv shell

version:
	pipenv run python --version
	pipenv run flake8 --version
	pipenv run pytest --version


###########################
# Start at local
###########################

start-local:
	# gunicorn

###########################
# Build and Deploy
###########################



