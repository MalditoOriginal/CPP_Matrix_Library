CC = gcc -Wall -Werror -Wextra -std=c++17 -pedantic -lstdc++
OS := $(shell uname)

ifeq ($(OS),Linux)
FLAGS = -lgtest -lm -lpthread -lrt -lsubunit -fprofile-arcs -ftest-coverage
else
FLAGS = -lgtest -lm -lpthread -fprofile-arcs -ftest-coverage
endif

all: test

clean:
	rm -rf *.o *.a test main
	rm -rf *.gcno *gcda *.gcov gcov
	rm -rf report report.info
	rm -rf *.dSYM

test: clean s21_matrix_oop.a
	$(CC) test.cpp s21_matrix_oop.a $(FLAGS) -o test
	./test

s21_matrix_oop.a: s21_matrix_oop.cpp 
	$(CC) -c s21_matrix_oop.cpp
	ar -crs s21_matrix_oop.a *.o

gcov_report: clean
	$(CC) test.cpp s21_matrix_oop.cpp  $(FLAGS) -o test
	./test
	lcov -t "./test" -o report.info --no-external -c -d . --ignore-errors mismatch
	genhtml -o report report.info
	open ./report/index.html
	rm -rf *.gcno *gcda *.gco

style_check:
	clang-format -style=Google -n *.cpp *.h

change_style:
	clang-format -style=Google -i *.cpp *.h

leak: clean test
	leaks -atExit -- ./test

.PHONY: all clean test gcov_report