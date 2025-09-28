* 상속의 기본 구조와 부모의 멤버 상속의 기본 구조와 부모의 멤버

        #include <iostream>
        using namespace std;
        
        class Vehicle
        {
        private:
            int person = 0;   // 탑승 인원
            int baggage = 0;  // 화물 무게
        public:
            void ride() {
                person++;
            } // 탑승
        
            void load(int weight) {
                baggage += weight;
            } // 짐 싣기
        
            void getOff() {
                person--;
            } // 하차
        
            int getPerson() {
                return person;
            } // 탑승 인원 확인
        };
        
        class Cruise : public Vehicle
        {
        private:
            int room;
        public:
            void setRoom(int room) {
                this->room = room;
            } // 크루즈의 방 수 설정
        
            void getRoom() {
                cout << room << endl;
            } // 크루즈의 방 수 확인
        
            void countPerson() {
                cout << Vehicle::getPerson() << endl;
            } // 부모 클래스 호출
        };
        
        class AirPlane : public Vehicle
        {
        private:
            int seat;
        public:
            void setSeat(int seat) {
                this->seat = seat;
            } // 자리 수 설정
        
            void getSeat() {
                cout << seat << endl;
            } // 자리 수 확인
        
            void countPerson() {
                cout << Vehicle::getPerson() << endl;
            } // 부모 클래스 호출
        };
        
        int main(int argc, char const* argv[])
        {
            Cruise dolphin;
            // ↘ 부모 클래스 멤버 함수 접근
            dolphin.ride();
            dolphin.load(10);
            // ↘ 자식 클래스 멤버 함수 호출
            dolphin.countPerson();
            dolphin.setRoom(100);
            dolphin.getRoom();
        
            AirPlane cppAir;
            // ↘ 부모 클래스 멤버 함수 접근
            cppAir.ride();
            cppAir.load(20);
            // ↘ 자식 클래스 멤버 함수 호출
            cppAir.countPerson();
            cppAir.setSeat(300);
            cppAir.getSeat();
        
            return 0;
        }


* 코드의 오류 위치를 통해, 상속 시 각각의 접근 지정자에 대한 차이를 실습

            #include <iostream>
            using namespace std;
            
            class Base {
            private:
                int a = 1;
            public:
                int b = 2;
            protected:
                int c = 3;
            };
            
            class Derived1 : public Base {
            public:
                void show() {
                    cout << "public" << endl;
                    cout << a << endl;   // 접근 불가 (private)
                    cout << b << endl;       // public 멤버 접근 가능
                    cout << c << endl;       // protected 멤버 접근 가능
                }
            };
            
            class Derived2 : private Base {
            public:
                void show() {
                    cout << "private" << endl;
                    cout << a << endl;   // 접근 불가 (private)
                    cout << b << endl;       // private 상속 → b도 private가 됨
                    cout << c << endl;       // private 상속 → c도 private가 됨
                }
            };
            
            class Derived3 : protected Base {
            public:
                void show() {
                    cout << "protected" << endl;
                    cout << a << endl;   // 접근 불가 (private)
                    cout << b << endl;       // protected 상속 → b는 protected
                    cout << c << endl;       // protected 상속 → c는 protected
                }
            };
            
            int main() {
                Derived1 obj1;
                Derived2 obj2;
                Derived3 obj3;
            
                obj1.show();
                obj2.show();
                obj3.show();
            
                main 함수에서 직접 접근 시도
                cout << obj1.a << endl;   // 접근 불가 (private)
                cout << obj1.b << endl;   // public 상속이라 접근 가능
                cout << obj1.c << endl;   // protected라 접근 불가
            
                cout << obj2.a << endl;   // 접근 불가
                cout << obj2.b << endl;   // private 상속이라 접근 불가
                cout << obj2.c << endl;   // private 상속이라 접근 불가
            
                cout << obj3.a << endl;   // 접근 불가
                cout << obj3.b << endl;   // protected 상속이라 접근 불가
                cout << obj3.c << endl;   // protected 상속이라 접근 불가
            
                return 0;
            }


* 부모와 자식의 동일한 이름의 함수는 어떻게 동작하는지에 대한 학습 (객체 슬라이싱)

                #include <iostream>
                using namespace std;
                
                class Animal {
                public:
                    void speak() {
                        cout << "동물이 소리를 냅니다." << endl;
                    }
                };
                
                class Dog : public Animal {
                public:
                    void speak() {
                        cout << "멍멍!" << endl;
                    }
                };
                
                class Cat : public Animal {
                public:
                    void speak() {
                        cout << "야옹!" << endl;
                    }
                };
                
                int main() {
                    Animal a1;
                    Dog a2;
                    Cat a3;
                
                    a1.speak();
                    a2.speak();
                    a3.speak();
                
                    Animal a;
                    a = a2;
                    a.speak();
                    a = a3;
                    a.speak();
                
                    return 0;
                }


* 추가 - 가상함수와 함수 재정의 (동적 다형성 : 오버라이딩)

                #include <iostream>
                using namespace std;
                
                class Animal {
                public:
                    // 부모 클래스의 speak를 가상 함수로 선언
                    virtual void speak() {
                        cout << "동물이 소리를 냅니다." << endl;
                    }
                };
                
                class Dog : public Animal {
                public:
                    // 부모의 virtual 함수를 override 키워드를 사용해서 재정의
                    void speak() override {
                        cout << "멍멍!" << endl;
                    }
                };
                
                class Cat : public Animal {
                public:
                    void speak() override {
                        cout << "야옹!" << endl;
                    }
                };
                
                int main() {
                    Animal a1;
                    Dog a2;
                    Cat a3;
                
                    a1.speak();
                    a2.speak();
                    a3.speak();
                
                    Animal* p;
                    p = &a2;
                    p->speak();
                
                    p = &a3;
                    p->speak();
                
                    Animal& r1 = a2;
                    Animal& r2 = a3;
                
                    r1.speak();
                    r2.speak();
                
                    return 0;
                }


* 상속과 생성자, 소멸자의 관계 - 기본적인 순서
 
                #include <iostream>
                using namespace std;
                
                class Parent {
                public:
                    Parent() {
                        cout << "부모 생성자 호출" << endl;
                    }
                    ~Parent() {
                        cout << "부모 소멸자 호출" << endl;
                    }
                };
                
                class Child : public Parent {
                public:
                    Child() {
                        cout << "자식 생성자 호출" << endl;
                    }
                    ~Child() {
                        cout << "자식 소멸자 호출" << endl;
                    }
                };
                
                int main() {
                    Child c;
                    return 0;
                }

* 상속과 생성자, 소멸자의 관계 - 부모 클래스 생성자에 파라미터가 있다면?

                #include <iostream>
                using namespace std;
                
                class Parent {
                public:
                    Parent(int x) {
                        cout << "부모 생성자 호출, 값 = " << x << endl;
                    }
                };
                
                class Child : public Parent {
                public:
                    // 자식 생성자에서 부모 생성자 직접 호출
                    Child(int y) : Parent(y) {
                        cout << "자식 생성자 호출" << endl;
                    }
                };
                
                int main() {
                    Child c(10);
                    return 0;
                }


* 다중 상속

                #include <iostream>
                using namespace std;
                
                class Engine {
                public:
                    void start() {
                        cout << "엔진 시동 걸림" << endl;
                    }
                };
                
                class Radio {
                public:
                    void playMusic() {
                        cout << "라디오 음악 재생" << endl;
                    }
                };
                
                // Car는 Engine과 Radio를 동시에 상속받음
                class Car : public Engine, public Radio {
                };
                
                int main() {
                    Car myCar;
                    myCar.start();      // Engine에서 물려받음
                    myCar.playMusic();  // Radio에서 물려받음
                
                    return 0;
                }


* 다형성 - 정적 다형성

                #include <iostream>
                using namespace std;
                
                void print(int x) {
                    cout << "정수 출력: " << x << endl;
                }
                
                void print(string s) {
                    cout << "문자열 출력: " << s << endl;
                }
                
                int main() {
                    print(10);        // 컴파일 시점에 int 버전 선택
                    print("Hello");   // 컴파일 시점에 string 버전 선택
                    return 0;
                }
