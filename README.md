#Сбор первоначальной инфы
Отмечу, все измерения были сделаны для работы 400 тиков программы

Пока соберем инфу, с помощью gpofng 
(Я хз как в md делать таблички, поэтому вот так)
| | |
 | -------------|------------------|
|Experiment      |test.1.er                                   |
|  Target        | './a.out'                                  |
|  Host          | HomeWork (x86_64, Linux 6.5.0-44-generic)  |
|  Start Time    | Wed Dec 25 00:01:42 2024                   |
|  Duration      | 263.552 Seconds                            |


Теперь посмотрим, чего больше всего потребляло   
| Коментарий | Данные | CPU % |
|---|----|---|
| Досутп к этому , вроде никак не улучшить |  std::array<std::pair<int, int>, 4ul>::data() const |    3.55 |
| Вот тута можно ускорить как будто )     | VectorField::get(int, int, int, int) | 3.68 |
 |Хз что это |    std::__invoke_result<std::identity&, std::pair<int, int> const&>::type std::__invoke<std::identity&, std::pair<int, int> const&>(std::identity&, std::pair<int, int> const&)  | 3.81 |
 |Тут тоже постараемся ускорить)   |    std::pair<int, int> const* std::ranges::__find_fn::operator()<std::pair<int, int> const*, std::pair<int, int> const*, std::pair<int, int>, std::identity>(std::pair<int, int> const*, std::pair<int, int> const*, std::pair<int, int> const&, std::identity) const  |  5.52 |
 |Тут тоже хз че это)    |   std::pair<int, int> const& std::forward<std::pair<int, int> const&>(std::remove_reference<std::pair<int, int> const&>::type&) |  7.75 |
 |Ускорим вывод)   | write | 11.04 |
 |Тут надо многопоточить)    |     propagate_flow(int, int, Fixed) | 11.70 |
 |Тут нельзя ускорить, оно и так быстрое)    |  bool std::operator==<int, int>(std::pair<int, int> const&, std::pair<int, int> const&) | 12.22 | 

Ради фана еще включим O3 и пару оптимизаций (заменим на printf, и прагмы)
| | |
|-|-|
|Experiment      |test.11.er                                  |
| Target        | './a.out'                                   |
| Host          | HomeWork (x86_64, Linux 6.5.0-44-generic)   |
| Start Time    | Wed Dec 25 01:10:28 2024                    |
| Duration      | 126.585 Seconds                             |

Ну давайте еще и разпаралледим (в процессе) 
