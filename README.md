Labs-1

    #include <iostream>
    #include <vector>
    using namespace std;
    int main(int argc, char const* argv[])
    {
    	vector <int> v1;
    
    	for (int i = 0; i < 10; i++) {
    		v1.push_back(i);
    
    		cout << "push_back: " << i << '\t'
    			<< "size: " << v1.size() << '\t'
    			<< "capacity: " << v1.capacity() << endl;
    	}
    }


Labs-2

    #include <vector>
    #include <iostream>
    using namespace std;
    int main()
    {
        vector<int> v1, v2, v3;
    
        v1.push_back(10);
        v1.push_back(20);
        v1.push_back(30);
        v1.push_back(40);
        v1.push_back(50);
    
        cout << "v1 = ";
        for (auto& v : v1) {
            cout << v << " ";
        }
        cout << endl;
    
        v2.assign(v1.begin(), v1.end());
        cout << "v2 = ";
        for (auto& v : v2) {
            cout << v << " ";
        }
        cout << endl;
    
        v3.assign(3, 6);
        cout << "v3 = ";
        for (auto& v : v3) {
            cout << v << " ";
        }
        cout << endl;
    
        v3.assign({ 5, 6, 7 });
        for (auto& v : v3) {
            cout << v << " ";
        }
        cout << endl;
    
        int& i = v1.at(0);
    
        cout << "v1 첫 번째 원소의 값:  " << i << endl;
    
        if (v1 == v2) cout << "v1과 v2는 같다." << endl;
        else cout << "v1과 v2는 다르다" << endl;
    
        i = 80;
        const int& j = v1.at(0);
    
        cout << "값을 변경 후 v1 첫 번째 원소의 값:  " << j << endl;
    
        if (v1 == v2) cout << "v1과 v2는 같다." << endl;
        else cout << "v1과 v2는 다르다" << endl;
    }


Labs-3

    #include <iostream>
    #include <vector>
    using namespace std;
    
    class Rect {
        int width;
        int height;
    
    public:
        Rect(int w, int h) {
            width = w;
            height = h;
        }
    
        int getWidth() {
            return width;
        }
        int getHeight() {
            return height;
        }
        int area() {
            return width * height;
        }
    
        void print() {
            cout << "가로: " << getWidth() << endl;
            cout << "세로: " << getHeight() << endl;
            cout << "넓이: " << area() << endl;
        }
    };
     
    int main() {
        vector<Rect> rects;
    
        rects.push_back(Rect(2, 5));
        rects.push_back(Rect(4, 4));
        rects.push_back(Rect(10, 6));
    
        for (auto& r : rects) {
            r.print();
        }
    
        int totalArea = 0;
        for (auto r : rects) {
            totalArea += r.area();
        }
        cout << "전체 사각형 넓이 합: " << totalArea << endl;
    }


Labs-4,5

    #include <vector>
    #include <iostream>
    using namespace std;
    int main()
    {
    	vector<int> v1(5);
    	v1.push_back(3);
    
    	cout << v1.capacity() << endl;
    	cout << v1.size() << endl;
    }


Labs-6

    #include <vector>
    #include <iostream>
    using namespace std;
    int main()
    {
    	vector<int> v1(5);
    	v1.push_back(3);
    	// v1[0] = 10;
    	// v1.at(0) = 10;
    	v1.insert(v1.begin(), 10);
    
    	cout << v1[0] << endl;
    	cout << v1.capacity() << endl;
    	cout << v1.size() << endl;
    }


Labs-7

    #include <iostream>
    #include <vector>
    using namespace std;
    
    void printVector(const vector<int>& v, const string& name)
    {
        cout << name << " = ";
        for (auto n : v)
            cout << n << " ";
        cout << endl;
    }
    
    int main()
    {
        // 테스트용 벡터 여러 개 준비
        vector<int> v1 = { 1, 2, 3 };
        vector<int> v2 = { 1, 2, 3 };
        vector<int> v3 = { 1, 2, 4 };
        vector<int> v4 = { 1, 2 };
        vector<int> v5 = { 1, 2, 3, 0 };
    
        printVector(v1, "v1");
        printVector(v2, "v2");
        printVector(v3, "v3");
        printVector(v4, "v4");
        printVector(v5, "v5");
        cout << endl;
    
        // 1) operator==
        cout << "[operator==]" << endl;
        cout << "v1 == v2 : " << (v1 == v2 ? "true" : "false") << endl; // true
        cout << "v1 == v3 : " << (v1 == v3 ? "true" : "false") << endl; // false
        cout << endl;
    
        // 2) operator!=
        cout << "[operator!=]" << endl;
        cout << "v1 != v2 : " << (v1 != v2 ? "true" : "false") << endl; // false
        cout << "v1 != v3 : " << (v1 != v3 ? "true" : "false") << endl; // true
        cout << endl;
    
        // 3) operator<
        cout << "[operator<]" << endl;
        cout << "v1 < v3 : " << (v1 < v3 ? "true" : "false") << endl; // true (3 < 4)
        cout << "v3 < v1 : " << (v3 < v1 ? "true" : "false") << endl; // false
        cout << "v4 < v1 : " << (v4 < v1 ? "true" : "false") << endl; // true (1,2 compared)
        cout << "v1 < v5 : " << (v1 < v5 ? "true" : "false") << endl; // true (3 < 3 → 길이 비교)
        cout << endl;
    
        // 4) operator<=
        cout << "[operator<=]" << endl;
        cout << "v1 <= v2 : " << (v1 <= v2 ? "true" : "false") << endl; // true (equal)
        cout << "v1 <= v3 : " << (v1 <= v3 ? "true" : "false") << endl; // true (3 < 4)
        cout << "v3 <= v1 : " << (v3 <= v1 ? "true" : "false") << endl; // false
        cout << endl;
    
        // 5) operator>
        cout << "[operator>]" << endl;
        cout << "v3 > v1 : " << (v3 > v1 ? "true" : "false") << endl; // true (4 > 3)
        cout << "v4 > v1 : " << (v4 > v1 ? "true" : "false") << endl; // false
        cout << endl;
    
        // 6) operator>=
        cout << "[operator>=]" << endl;
        cout << "v1 >= v2 : " << (v1 >= v2 ? "true" : "false") << endl; // true
        cout << "v3 >= v1 : " << (v3 >= v1 ? "true" : "false") << endl; // true
        cout << "v4 >= v1 : " << (v4 >= v1 ? "true" : "false") << endl; // false
        cout << endl;
    
        return 0;
    }


Labs-8

    #include <iostream>
    #include <vector>
    using namespace std;
    
    int main() {
        vector<int> v = { 10, 20, 30, 40 };
    
        // back() 검증
        cout << "[backTest]" << endl;
        for (auto n : v) cout << n << " ";
        cout << endl;
        cout << "back()으로 마지막 요소 접근 / 마지막 요소: " << v.back() << endl;
        v.back() = 999;
        cout << "back()으로 마지막 값 변경 / 마지막 요소: " << v.back() << endl << endl;
    
        // begin() 검증
        cout << "[beginTest]" << endl;
        for (auto n : v) cout << n << " ";
        cout << endl;
        cout << "begin()으로 첫 번째 요소 접근 / 첫 번째 요소: " << *v.begin() << endl;
        *v.begin() = 111;
        cout << "begin()으로 첫 번째 요소 접근 / 첫 번째 요소: " << *v.begin() << endl << endl;    
    
        // end() 검증
        cout << "[endTest]" << endl;
        for (auto it = v.begin(); it != v.end(); it++) {
            cout << *it << " ";
        }
        cout << endl << endl;
    
        // clear() 검증
        cout << "[clearTest]" << endl;
        cout << "clear() 호출 전 요소 및 size" << endl;
        for (auto n : v) cout << n << " ";
        cout << '\t' << "size: " << v.size() << endl;
        v.clear();
        cout << "clear() 호출 후 요소 및 size" << endl;
        for (auto n : v) cout << n << " ";
        cout << '\t' << "size: " << v.size() << endl;
    }
