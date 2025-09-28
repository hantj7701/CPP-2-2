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


  * 
