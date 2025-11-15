* 서버 스크립트
  // server.cpp : 간단한 TCP 파일 전송 서버
  // 빌드: cl /EHsc server.cpp ws2_32.lib
  
  #define _WINSOCK_DEPRECATED_NO_WARNINGS
  #include <winsock2.h>
  #include <iostream>
  #include <fstream>
  #include <string>
  
  #pragma comment(lib, "ws2_32.lib")
  
  int main() {
      WSADATA wsaData;
      WSAStartup(MAKEWORD(2, 2), &wsaData); // WinSock 초기화
  
      SOCKET serverSock = socket(AF_INET, SOCK_STREAM, 0); // TCP 소켓 생성
  
      sockaddr_in serverAddr;
      serverAddr.sin_family = AF_INET;
      serverAddr.sin_addr.s_addr = INADDR_ANY;  // 모든 IP에서 접속 허용
      serverAddr.sin_port = htons(9000);        // 포트 번호 9000 사용
  
      // 소켓에 주소 바인딩
      bind(serverSock, (SOCKADDR*)&serverAddr, sizeof(serverAddr));
  
      // 클라이언트 접속 대기
      listen(serverSock, 1);
      std::cout << "[서버] 클라이언트 접속 대기 중...\n";
  
      sockaddr_in clientAddr;
      int clientAddrSize = sizeof(clientAddr);
      SOCKET clientSock = accept(serverSock, (SOCKADDR*)&clientAddr, &clientAddrSize);
      std::cout << "[서버] 클라이언트 접속!\n";
  
      // 서버에 있는 파일 이름 4개를 배열로 저장
      const char* fileList[] = {
          "A.txt",
          "B.txt",
          "C.png",
          "D.png"
      };
      const int fileCount = sizeof(fileList) / sizeof(fileList[0]);
      char cmdBuf[256];
      int recvLen;
  
      // 명령 루프
      while (true) {
          // 클라이언트로부터 명령 수신
          recvLen = recv(clientSock, cmdBuf, sizeof(cmdBuf) - 1, 0);
          if (recvLen <= 0) {
              std::cout << "[서버] 클라이언트 연결 종료 \n";
              break;
          }
          cmdBuf[recvLen] = '\0'; // 문자열로 사용하기 위해 널 문자 추가
  
          std::cout << "[서버] 명령 수신: " << cmdBuf << "\n";
  
          // list 명령 처리
          if (std::strncmp(cmdBuf, "list", 4) == 0 && cmdBuf[4] == '\0') {
              // 파일 목록 문자열 만들기
              std::string listStr;
              for (int i = 0; i < fileCount; ++i) {
                  listStr += fileList[i];
                  listStr += "\n";
              }
              
              send(clientSock, listStr.c_str(), (int)listStr.size(), 0);
              std::cout << "[서버] 파일 목록 전송 완료. \n";
          }
  
          // get 명령 처리
          else if (std::strncmp(cmdBuf, "get ", 4) == 0) {
              const char* fileName = cmdBuf + 4; // "get " 뒤부터가 파일명
  
              std::cout << "[서버] 파일 전송 요청: " << fileName << "\n";
  
              std::ifstream file(fileName, std::ios::binary);
              if (!file) {
                  std::cerr << "[서버] " << fileName << " 파일을 열 수 없습니다.\n";
                  const char* errMsg = "ERROR: cannot open file";
                  send(clientSock, errMsg, (int)std::strlen(errMsg), 0);
                  continue;
              }
  
              // 파일 내용을 읽어서 클라이언트로 전송
              char buffer[1024];
              while (!file.eof()) {
                  file.read(buffer, sizeof(buffer));
                  int bytesRead = file.gcount();
                  if (bytesRead > 0) {
                      send(clientSock, buffer, bytesRead, 0);
                  }
              }
              // 정리
              std::cout << "[서버] 파일 전송 완료!\n";
              file.close();
              break;
          }
          else {
              std::cout << "[서버] 알 수 없는 명령. \n";
          }
      }
  
      closesocket(clientSock);
      closesocket(serverSock);
      WSACleanup();
  
      return 0;
  }
* 클라이언트 스크립트
  // client.cpp : 간단한 TCP 파일 수신 클라이언트
  // 빌드: cl /EHsc client.cpp ws2_32.lib
  
  #define _WINSOCK_DEPRECATED_NO_WARNINGS
  #include <winsock2.h>
  #include <iostream>
  #include <fstream>
  #include <string>
  
  #pragma comment(lib, "ws2_32.lib")
  
  int main() {
      WSADATA wsaData;
      WSAStartup(MAKEWORD(2, 2), &wsaData); // WinSock 초기화
  
      SOCKET clientSock = socket(AF_INET, SOCK_STREAM, 0); // TCP 소켓 생성
  
      sockaddr_in serverAddr;
      serverAddr.sin_family = AF_INET;
      serverAddr.sin_addr.s_addr = inet_addr("127.0.0.1"); // 서버 주소 (로컬)
      serverAddr.sin_port = htons(9000);                   // 서버 포트
  
      // 서버에 연결 시도
      if (connect(clientSock, (SOCKADDR*)&serverAddr, sizeof(serverAddr)) == SOCKET_ERROR) {
          std::cerr << "[클라이언트] 서버에 연결할 수 없습니다.\n";
          closesocket(clientSock);
          WSACleanup();
          return 1;
      }
  
      std::cout << "[클라이언트] 서버에 연결되었습니다.\n";
  
      std::string cmd;
  
      std::cout << "list 입력 시 파일 목록을 제공합니다!\n>";
      std::getline(std::cin, cmd);
  
      if (cmd == "list") {
          // 서버에게 list 전송
          send(clientSock, cmd.c_str(), (int)cmd.size(), 0);
  
          // 서버에서 파일 목록 수신
          char listBuf[1024];
          int len = recv(clientSock, listBuf, sizeof(listBuf) - 1, 0);
          if (len > 0) {
              listBuf[len] = '\0';
              std::cout << "\n[클라이언트] 서버 파일 목록 : \n";
              std::cout << listBuf << "\n";
          }
      }
  
      // get 명령으로 특정 파일 요청
      std::cout << "전달받을 파일을 선택하여 'get 파일명' 형식으로 입력해주세요! Ex) get A.txt\n>";
      std::getline(std::cin, cmd);
  
      // get으로 시작하는지 검사
      if (cmd.rfind("get ", 0) == 0) {
          // 서버로 get 명령 전송
          send(clientSock, cmd.c_str(), (int)cmd.size(), 0);
  
          // 요청한 파일명 확인 및 생성 준비
          std::string fileName = cmd.substr(4);
          std::string outName = "received_" + fileName;
  
          // 서버가 전송한 파일 내용 저장
          std::ofstream outFile(outName, std::ios::binary);
          if (!outFile) {
              std::cerr << "[클라이언트] 출력 파일을 생성할 수 없습니다.\n";
          }
          else {
              char buffer[1024];
              int bytesReceived;
  
              // 서버가 소켓을 닫을 때까지 계속 읽기
              while ((bytesReceived = recv(clientSock, buffer, sizeof(buffer), 0)) > 0) {
                  outFile.write(buffer, bytesReceived);
              }
  
              std::cout << "[클라이언트] " << outName << " 파일 수신 완료!\n";
              outFile.close();
          }
      }
      else {
          std::cout << "[클라이언트] get 명령이 올바르지 않습니다.\n";
      }
  
      // 정리
      closesocket(clientSock);
      WSACleanup();
  
      return 0;
  }
