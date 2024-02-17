#include <iostream>
#include <fstream>
#include <chrono>
#include <ctime>
#include <random>
#include <string>
#include <sstream>
#include <vector>
#include <iomanip>
#include <openssl/opensslv.h>
#include <openssl/ssl.h>
#include <openssl/sha.h>


// Функция для вычисления хеша данных
std::string calculateHash(const std::string& data) {
    unsigned char hash[SHA256_DIGEST_LENGTH];
    SHA256_CTX sha256;
    SHA256_Init(&sha256);
    SHA256_Update(&sha256, data.c_str(), data.length());
    SHA256_Final(hash, &sha256);

    std::stringstream ss;
    for (int i = 0; i < SHA256_DIGEST_LENGTH; i++) {
        ss << std::hex << (int)hash[i];
    }

    return ss.str();
}

int main(int argc, char* argv[]) {
    std::string filename;
    if (argc > 1) {
        filename = argv[1];
    }
    else {
        std::cout << "Введите имя файла: ";
        std::cin >> filename;
    }

    std::ifstream file(filename);
    if (!file.is_open()) {
        std::cerr << "Ошибка открытия файла." << std::endl;
        return 1;
    }

    std::string data((std::istreambuf_iterator<char>(file)), std::istreambuf_iterator<char>());
    file.close();

    std::string hash = calculateHash(data);

    auto now = std::chrono::system_clock::now();
    std::time_t now_time = std::chrono::system_clock::to_time_t(now);

    std::stringstream timestamp_ss;
    timestamp_ss << std::put_time(std::localtime(&now_time), "%F %T");
    std::string timestamp = timestamp_ss.str();

    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<> dis(1000, 9999);
    int random_number = dis(gen);

    hash += timestamp + std::to_string(random_number);

    std::cout << "Цифровая подпись данных: " << hash << std::endl;

    std::string output_filename = "signature_" + filename;
    std::ofstream output_file(output_filename);
    if (output_file.is_open()) {
        output_file << hash << std::endl << data;
        output_file.close();
        std::cout << "Цифровая подпись сохранена в файле: " << output_filename << std::endl;
    }
    else {
        std::cerr << "Ошибка открытия файла для сохранения цифровой подписи." << std::endl;
        return 1;
    }

    return 0;
}


