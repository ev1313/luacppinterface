project(LuaCppInterface)
cmake_minimum_required(VERSION 3.0)

find_package(Lua REQUIRED)

option(BUILD_TESTS "build tests" OFF)

function(setup_target NAME)
  set_property(TARGET ${NAME} PROPERTY CXX_STANDARD 14)
  set_property(TARGET ${NAME} PROPERTY CXX_STANDARD_REQUIRED ON)
  target_compile_options(${NAME} PRIVATE -Wall -Wextra -ggdb -O2)
endfunction(setup_target)

function(create_test NAME)
  add_executable( ${NAME} tests/${NAME}.cpp )
  setup_target( ${NAME} )
  target_link_libraries( ${NAME} ${LUA_LIBRARIES} )
  target_link_libraries( ${NAME} luacppinterface )
endfunction(create_test)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}\\bin)

include_directories(src)
include_directories(${LUA_INCLUDE_DIR})

add_library(luacppinterface STATIC 
  src/luacoroutine
  src/luareference
  src/luacppinterface
  src/luatable
  src/luafunction
  src/luaerror
  src/luastringconversion
)

IF(BUILD_TESTS)

set(TEST_NAMES
  demonstration1
  demonstration2
  demonstration3
  demonstration4
  luacpp
  testcallabletable
  testchartowideconvert
  testcoroutineexception
  testcoroutineexpectedbutnot
  testcreate
  testctoluafunction
  testexceptioninluafunction
  testextractingasfunctionbutistable
  testfunction
  testfunctionerror
  testfunctionexpectedbutnotcallable
  testfunctionparams
  testfunctionparamsreturn
  testfunctionreturn
  testgetmetatableonclosedstate
  testgetsetfunction2
  testgetsetfunction
  testgetsetinsidetable
  testgetsetinteger
  testgetsetstring
  testgetsettable
  testgetstate
  testgettypeofvalueat
  testinvalidscript
  testpassingfunction
  testreadinglightuserdatafromuserdata
  testregistry
  testresumenoscript
  testreturnfromnative
  testreturnfromscript
  testreturnfromyieldingfunction
  testreturntablefromlua
  testreturntablefromnative
  testscriptexception
  testsetstring
  testsetwstring
  testtable
  testtableexpectedbutnotable
  testtableforeachintegerkey
  testtableforeachstringkey
  testtypeintstringmorph
  testtypemorphintwstring
  testtypestringintmorph
  testuserdataconstructor
  testuserdata
  testuserdatadispose
  testuserdatamethod
  testwidetocharconvert
)

foreach( TEST_NAME ${TEST_NAMES} )
  create_test(${TEST_NAME})
endforeach( TEST_NAME ${TEST_NAMES} )

ENDIF(BUILD_TESTS)
