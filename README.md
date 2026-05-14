## Домашняя работа из Лабораторной работы №1
В рамках выполнения данной лабораторной работы мною были выполнены команды из tutorial с некоторыми изменениями:
1) Скачаем библиотеку boost с помощью wget:
```bash
$ cd ~
$ wget https://sourceforge.net/projects/boost/files/boost/1.69.0/boost_1_69_0.tar.gz

2026-05-14 11:36:08 (1,28 MB/s) - «boost_1_69_0.tar.gz» сохранён [111710205/111710205]
```
2) Разархивируйте скачанный файл в директорию ~/boost_1_69_0, файл распакуется в папку boost_1_69_0
```bash
$ tar -xf boost_1_69_0.tar.gz
```
3) Подсчитаем количество файлов НЕ включая вложенные директории:
```bash
$ find . -maxdepth 1 -type f | wc -l
12
```
4) Подсчитаем количество файлов ВКЛЮЧАЯ вложенные директории:
```bash
$ find . -type f | wc -l
61191
```
5.1) Подсчитаем количество заголовочных файлов (.hpp, .h):
```bash
$ find . -type f \( -name "*.hpp" -o -name "*.h" \) | wc -l
15208
```
5.2) Подсчитаем количество файлов .cpp:
```bash
$ find . -type f -name "*.cpp" | wc -l
13774
```
5.3) Подсчитаем количество остальных файлов:
```bash
$ find . -type f -not -name "*.hpp" -not -name "*.h" -not -name "*.cpp" | wc -l
32209
```
6) Найдем полный путь до файла any.hpp:
```bash
$ find ~/boost_1_69_0 -name "any.hpp"
/home/kristina/boost_1_69_0/boost/xpressive/detail/utility/any.hpp
/home/kristina/boost_1_69_0/boost/type_erasure/any.hpp
/home/kristina/boost_1_69_0/boost/hana/fwd/any.hpp
/home/kristina/boost_1_69_0/boost/hana/any.hpp
/home/kristina/boost_1_69_0/boost/proto/detail/any.hpp
/home/kristina/boost_1_69_0/boost/spirit/home/support/algorithm/any.hpp
/home/kristina/boost_1_69_0/boost/any.hpp
/home/kristina/boost_1_69_0/boost/fusion/algorithm/query/detail/any.hpp
/home/kristina/boost_1_69_0/boost/fusion/algorithm/query/any.hpp
/home/kristina/boost_1_69_0/boost/fusion/include/any.hpp
```
7) Выведите все файлы, где упоминается последовательность boost::asio:
```bash
$ grep -r "boost::asio" --include="*.hpp" --include="*.cpp" --include="*.h" ~/boost_1_69_0 | cut -d: -f1 | sort -u
/home/kristina/boost_1_69_0/boost/asio/async_result.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_datagram_socket.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_deadline_timer.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_io_object.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_raw_socket.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_seq_packet_socket.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_serial_port.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_signal_set.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_socket_acceptor.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_socket.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_socket_iostream.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_socket_streambuf.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_streambuf.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_stream_socket.hpp
/home/kristina/boost_1_69_0/boost/asio/basic_waitable_timer.hpp
/home/kristina/boost_1_69_0/boost/asio/buffered_read_stream.hpp
/home/kristina/boost_1_69_0/boost/asio/buffered_stream.hpp
/home/kristina/boost_1_69_0/boost/asio/buffered_write_stream.hpp
/home/kristina/boost_1_69_0/boost/asio/buffer.hpp
/home/kristina/boost_1_69_0/boost/asio/buffers_iterator.hpp
/home/kristina/boost_1_69_0/boost/asio/completion_condition.hpp ......и т.д.
```
8) Скомпилируем boost:
```bash
cd ~/boost_1_69_0
```
Сначала нужно собрать инструмент b2 (bjam):
```bash
$ ./bootstrap.sh --prefix=~/boost-build
Building Boost.Build engine with toolset gcc... tools/build/src/engine/bin.linuxx86_64/b2
Unicode/ICU support for Boost.Regex?... not found.
Generating Boost.Build configuration in project-config.jam...

Bootstrapping is done. To build, run:

    ./b2

To adjust configuration, edit 'project-config.jam'.
Further information:

   - Command line help:
     ./b2 --help

   - Getting started guide:
     http://www.boost.org/more/getting_started/unix-variants.html

   - Boost.Build documentation:
     http://www.boost.org/build/doc/html/index.html

```
И запустим ускоренную сборку только нужных библиотек:
```bash
$ ./b2 --with-system --with-filesystem --with-thread --with-date_time
```
9) Перенесем все скомпилированные статические библиотеки в директорию ~/boost-libs:
```bash
$ mkdir -p ~/boost-libs
$ find ~/boost_1_69_0/stage/lib -name "*.a" -exec cp {} ~/boost-libs/ \;
```
10) Подсчитаем, сколько дискового пространства занимает каждый файл:
```bash
$ cd ~/boost-libs
$ ls -lh *.a
-rw-rw-r-- 1 kristina kristina 149K мая 14 15:28 libboost_date_time.a
-rw-rw-r-- 1 kristina kristina 400K мая 14 15:28 libboost_filesystem.a
-rw-rw-r-- 1 kristina kristina 1,3K мая 14 15:28 libboost_system.a
```
11) Найдем топ-10 самых "тяжёлых" файлов:
```bash
$ ls -lhS *.a | head -10
-rw-rw-r-- 1 kristina kristina 400K мая 14 15:28 libboost_filesystem.a
-rw-rw-r-- 1 kristina kristina 149K мая 14 15:28 libboost_date_time.a
-rw-rw-r-- 1 kristina kristina 1,3K мая 14 15:28 libboost_system.a
```
