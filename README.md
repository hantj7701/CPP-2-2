낭나난낭

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
