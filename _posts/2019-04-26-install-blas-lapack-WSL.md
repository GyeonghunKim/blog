---
title: "WSL, CLion 환경에서 BLAS, LAPACK 설치하기"
categories: 환경구축
tags:
  - WSL
  - Window10
  - Ubuntu
  - 윈도우
  - 수치해석
  - 선형대수
  - BLAS 
  - LAPACK
  - LAPACKE
  - Linear Algebra
  - Eigenvalue

last_modified_at: 2019-04-26T13:00:00+09:00
toc: true 
toc_label: "Table of Contents"
toc_icon: "cog" 
toc_sticky: true 
author_profile: true
comments: true

gallery1: 
  - url: /assets/images/WSL_install/007.PNG
    image_path: /assets/images/WSL_install/007.PNG
    alt: "placeholder image "
    title: "Image 1 title caption"

---


## 0. Preliminary
이후의 내용은 윈도우에 WSL을 이용하여 Ubuntu 18.04가 설치되어있고, CLion으로 연동되어 있는 환경에 대한 설명입니다! Native Ubuntu에 대해서도 아래의 설명대로 하면 쉽게 설치할 수 있을 것이나, 윈도우 환경에서는 아래의 WSL 설치 과정을 먼저 진행해주세요!

[Window에 WSL설치하기](https://gyeonghunkim.github.io/blog/%ED%99%98%EA%B2%BD%EA%B5%AC%EC%B6%95/install-WSL/)

[WSL 컴파일러 윈도우에서 사용하기 (with CLion)](https://gyeonghunkim.github.io/blog/%ED%99%98%EA%B2%BD%EA%B5%AC%EC%B6%95/WSL-Clion/)

## 1. BLAS? LAPACK?
[BLAS](http://www.netlib.org/blas/)와 [LAPACK](http://www.netlib.org/lapack/)은 각각 선형대수를 위한 Fortran library입니다. 이를 컴파일 된 함수를 불러와 C++에서 사용할 수 있습니다. LAPACK과 BLAS를 이용하는 이유는 직접 사용했을 때, 익숙해지면 행렬 연산을 쉽게 할 수 있다는 것도 있지만, 이걸 기반으로 만들어진 다른 프로그램을 사용하는데에도 필요합니다.  
BLAS는 기초적인 행렬의 연산을 할 수 있도록 만들어진 library입니다. 아래의 사진에서 볼 수 있듯이, 행렬 곱하기, transpose등을 할 수 있습니다. 이를 이용하여 코드를 작성할 때는 아래 사진의 level 1, 2, 3을 적절하게 이용하는 것이 좋습니다. 

{% include gallery id="gallery3" %}

LAPACK은 BLAS보다 고급 행렬 계산에 사용됩니다. 주로 선형 방정식을 풀거나 eigenvalue를 구하는 등에 사용됩니다. LAPACK에서 사용할 수 있는 루틴(함수)들은 [여기](http://www.icl.utk.edu/~mgates3/docs/lapack.html)에서 확인할 수 있습니다. 


## 2. BLAS, LAPACK 설치하기
윈도우에서 BLAS와 LAPACK을 설치하고 링킹하기 매우 힘들지만, WSL과 CLion(또는 CMake)와 함께라면 매우 간단하게 가능합니다! 첫 단계로 진행할 것은 WSL에 BLAS와 LAPACK을 설치하는 것이고, 두 번째 단계는 BLAS, LAPACK 사용을 위해서 CMakelist를 수정하는 것 입니다. 

### 1. WSL에 BLAS, LAPACK 설치하기
WSL은 Ubuntu 환경이기 때문에 BLAS와 LAPACK을 간단하게 설치할 수 있습니다. 우선 아래의 명령어로 gfortran을 설치해줍니다.   
```bash
sudo apt-get install gfortran  
```
이후에 아래와 같이 BLAS와 LAPACK을 설치할 수 있습니다. 

```bash
sudo apt-get install libblas-dev liblapack-dev
```

이제 설치가 끝났습니다! 매우 간단해서 좋습니다. 

### 2. CMakelist를 수정하기
CLion에서 File > New Project를 눌러서 새 프로젝트를 열어줍니다. 저는 프로젝트 이름을 "blas_lapack_test"로 했습니다. 

{% include gallery id="gallery1" %}

그러면 아래 화면과 같이 main.cpp와 CMakeList.txt가 자동으로 형성된 것을 볼 수 있습니다. 

{% include gallery id="gallery2" %}

이제 CMakeList.txt를 아래와 같이 바꾸어 줍니다. 

```cmake 
cmake_minimum_required(VERSION 3.10)

project(blas_lapack_test)

set(CMAKE_CXX_STANDARD 14)

add_executable(blas_lapack_test main.cpp)

FIND_PACKAGE(LAPACK)
FIND_PACKAGE(BLAS)
target_link_libraries(BLASLAPACKTEST ${BLAS_LIBRARIES} ${LAPACK_LIBRARIES})
```
각 줄에 대한 설명을 적어보면, **cmake_minimum_required()**는 이 CMakeList.txt를 이용하여 사용할 최소한의 CMake version에 대한 정보를 말합니다. 설치되어 있는 CMake의 버전이 이 값보다 작으면 에러가 발생합니다. 두 번째 줄은 project의 이름을 지정하는 것 입니다. 세 번째 출은 이 프로젝트에서 사용할 C++ compiler의 버전을 지정해 줄 수 있습니다. 그 다음줄의 add_excuatable은 사용할 코드를 지정해주는 것입니다. 첫 입력은 프로젝트의 이름, 두 번째 부터는 코드들의 파일 명 입니다. 만약 test_header.h를 사용할 예정이라면 
```cmake
add_executable(blas_lapack_test main.cpp test_header.h)
```
와 같이 사용할 수 있습니다.   

그 다음 줄 부터 본격적으로 LAPACK와 BLAS를 불러오는 작업입니다. FIND_PACKAGE는 설치되어있는 패키지를 찾아주는 명령입니다. 우리의 코드에서는 LAPACK와 BLAS를 찾아준 것이 됩니다. 그 후에 target_link_libraries가 본격적으로 우리의 코드에 LAPACK과 BLAS를 링킹하라는 명령입니다. 

이제 BLAS와 LAPACK을 사용하기 위한 준비가 모두 끝났습니다!

## 3. BLAS와 LAPACK을 이용하기!
이제 위에서 만든 프로젝트 파일에 main.cpp를 수정해서 BLAS와 LAPACK을 실행해보는 예제를 만들어 보겠습니다!   


BLAS 예제는 [이곳](https://ubuntuforums.org/archive/index.php/t-1740797.html)을, LAPACK 예제는 [Wotao Yin님의 페이지](http://www.math.ucla.edu/~wotaoyin/)를 참조했습니다.   

BLAS의 예제로는 두 벡터의 내적을 해주는 함수인 ddot_을 이용해 볼 것이고, LAPACK의 예제로는 A를 LU factorization하는 dgetrf와 이렇게 factorization한 A를 이용하여 선형 시스템 Ax = b를 풀어주는 dgetfs를 사용해 보도록 하겠습니다. 다양한 함수들에 대해서는 추후의 글에서 다루겠습니다. 코드는 아래와 같습니다. 

```C++
#include <iostream>
#include <vector>
// from BLAS
extern "C" double ddot_( const int *N, const double *a, const int *inca, const double *b, const int *incb );
// from LAPACK
extern "C" void dgetrf_(int* dim1, int* dim2, double* a, int* lda, int* ipiv, int* info);
extern "C" void dgetrs_(char *TRANS, int *N, int *NRHS, double *A, int *LDA, int *IPIV, double *B, int *LDB, int *INFO );

void test_BLAS(){
    std::vector<double> a, b;
    a.push_back(1.0);
    a.push_back(2.1);
    a.push_back(3.6);
    b.push_back(2.2);
    b.push_back(2.1);
    b.push_back(3.1);
    int N = 3, one = 1;
    double dot_product = ddot_( &N, & *a.begin(), &one, & *b.begin(), &one );
    printf(" The dot product is: %f \n",dot_product );
}
void test_LAPACK(){
    char trans = 'N';
    int dim = 2;
    int nrhs = 1;
    int LDA = dim;
    int LDB = dim;
    int info;

    std::vector<double> a, b;

    a.push_back(1.21);
    a.push_back(2.3);
    a.push_back(1.3);
    a.push_back(-1);

    b.push_back(2);
    b.push_back(34.1);

    int ipiv[3];

    dgetrf_(&dim, &dim, &*a.begin(), &LDA, ipiv, &info);
    dgetrs_(&trans, &dim, &nrhs, & *a.begin(), &LDA, ipiv, & *b.begin(), &LDB, &info);


    std::cout << "solution is:";
    std::cout << "[" << b[0] << ", " << b[1] << ", " << "]" << std::endl;
}
int main()
{
    std::cout << "*********** this is from BLAS_TEST ****************" << std::endl;
    test_BLAS();
    std::cout << "*********** this is from LAPACK_TEST ****************" << std::endl;
    test_LAPACK();
    return(0);
}
```

앞의 extern "C"로 시작하는 세 줄은 외부 라이브러리를 가져오는 C++의 문법입니다. C++ 컴파일러에게 C 스타일의 linkage를 사용하라고 말해주는 내용인데, 단순하게 함수를 가져온다고 생각해도 사용하는데는 문제 없는 것 같습니다. ddot_는 BLAS에서 가져온 내적을 하는 함수이고, dgetrf_와 dgetrs_는 각각 LAPACK에서 가져온 LU decomposition을 하는 함수와 그 결과를 이용하여 Ax = b를 푸는 함수입니다. 


