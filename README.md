[![Coverage Status](https://img.shields.io/badge/coverage-100%25-ff69b4)](https://coveralls.io/github/ReZooty/lab06)
# lab06 by Telepov Igor
# Создание теста для Account
```sh
rezooty@Katana-GF76-11UE:~/ReZooty/workspace/projects/lab06v2/tests cat > TEST1_account.cpp
#include "Account.h"

#include <gtest/gtest.h>
#include <gmock/gmock.h>

class AccountMock : public Account {
public:
    AccountMock(int id, int balance) : Account(id, balance) {}
    MOCK_CONST_METHOD0(GetBalance, int());
    MOCK_METHOD1(ChangeBalance, void(int diff));
    MOCK_METHOD0(Lock, void());
    MOCK_METHOD0(Unlock, void());
};

TEST(Account, Mock) {
    AccountMock acc(1, 666);
    EXPECT_CALL(acc, GetBalance()).Times(1);
    EXPECT_CALL(acc, ChangeBalance(testing::_)).Times(2);
    EXPECT_CALL(acc, Lock()).Times(2);
    EXPECT_CALL(acc, Unlock()).Times(1);
    acc.GetBalance();
    acc.ChangeBalance(100);
    acc.Lock();
    acc.ChangeBalance(100);
    acc.Lock();
    acc.Unlock();
}

TEST(Account, SimpleTest) {
    Account acc(1, 666);
    EXPECT_EQ(acc.id(), 1);
    EXPECT_EQ(acc.GetBalance(), 666);
    EXPECT_THROW(acc.ChangeBalance(200), std::runtime_error);
    EXPECT_NO_THROW(acc.Lock());
    acc.ChangeBalance(200);
    EXPECT_EQ(acc.GetBalance(), 866);
    EXPECT_THROW(acc.Lock(), std::runtime_error);
    EXPECT_NO_THROW(acc.Unlock());
}

```
# Создание теста для Transaction
```sh
rezooty@Katana-GF76-11UE:~/ReZooty/workspace/projects/lab06v2/tests cat > TEST2_transaction.cpp
#include "Transaction.h"
#include "Account.h"

#include <gtest/gtest.h>
#include <gmock/gmock.h>

class TransactionMock : public Transaction {
public:
    MOCK_METHOD3(Make, bool(Account& from, Account& to, int sum));
};

TEST(Transaction, Mock) {
    TransactionMock tr;
    Account account_1(1, 100);
    Account account_2(2, 300);
    EXPECT_CALL(tr, Make(testing::_, testing::_, testing::_))
        .Times(5);
    tr.set_fee(200);
    tr.Make(account_1, account_2, 200);
    tr.Make(account_2, account_1, 300);
    tr.Make(account_1, account_1, 0);
    tr.Make(account_1, account_2, -5);
    tr.Make(account_2, account_1, 50);
}

TEST(Transaction, SimpleTest) {
    Transaction tr;
    Account account_1(1, 100);
    Account account_2(2, 300);
    tr.set_fee(10);
    EXPECT_EQ(tr.fee(), 10);
    EXPECT_THROW(tr.Make(account_1, account_2, 40), std::logic_error);
    EXPECT_THROW(tr.Make(account_1, account_2, -5), std::invalid_argument);
    EXPECT_THROW(tr.Make(account_1, account_1, 100), std::logic_error);
    EXPECT_FALSE(tr.Make(account_1, account_2, 400));
    EXPECT_FALSE(tr.Make(account_2, account_1, 300));
    EXPECT_FALSE(tr.Make(account_2, account_1, 290));
    EXPECT_TRUE(tr.Make(account_2, account_1, 150));
}

```
# CMakeLists
```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TESTS "Build tests" OFF)

if(BUILD_TESTS)
  add_compile_options(--coverage)
endif()

project (banking)

add_library(banking STATIC ${CMAKE_CURRENT_SOURCE_DIR}/banking/Transaction.cpp ${CMAKE_CURRENT_SOURCE_DIR}/banking/Account.cpp)
target_include_directories(banking PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/banking )

target_link_libraries(banking gcov)

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(googletest)
  file(GLOB BANKING_TEST_SOURCES tests/*.cpp)
  add_executable(check ${BANKING_TEST_SOURCES})
  target_link_libraries(check banking gtest_main gmock_main)
  add_test(NAME check COMMAND check)
endif()
```
