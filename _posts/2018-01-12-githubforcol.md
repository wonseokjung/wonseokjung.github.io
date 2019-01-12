---
title: "github for collaboration"
date: 2018-01-12 12:26:28 -0400
categories: Reinforcementlearning update
use_math : true
---




# 협업을 위한 git 

# 전체 Process

1. Local git으로 가져오기 ( 처음 : clone 이후 : pull ) 
2. Branch 만드는 작업
3. 기능 추가 
4. Test 코드 만들고
5. commit
6. push
7. pull request 
8. Reviewer
9. Assign 

## Process 1
Local git으로 가져오기

    git pull https://github.com/kairproject/practice_collaboration.git
    
    git remote -v
    origin	https://github.com/kairproject/practice_collaboration.git (fetch)
    origin	https://github.com/kairproject/practice_collaboration.git (push)


## process2
branch 만드는 방법

## branch 지우는 명령어

git checkout → branch를 바꾸는 명령어 

git checkout -b name → branch를 만들면서 그쪽으로 옮겨간다. 

    git branch -d branchname

어떠한 branch를 보유하고 있는지 체크하는 명령어 

    git branch

상태 체크 

    git status

## Process 3
기능 추가

코드에 function 추가했다. 

![](https://www.dropbox.com/s/wmvz2nk7ezybxvv/Screenshot%202019-01-12%2013.39.37.png?raw=1)

## class 와 function 설명

    """Simple calculator example for the practice of collaboration."""


​    
    class Calculator(object):
    
    		# class에 대한 설명
        """Simple calculator which has 4 functionalities.
    
        => add, sub, mul, div
        => bitwise_shift_left, bitwise_shift_right, bitwise_and
        => bitwise_or, bitwise_not, bitwise_xor
    
        All methods will be implemented as staticmethods
        so that it can be called without making an object.
    
        !!Don't forget to run static analyzers and unittests
        before pushing your code. (check 100% test covered)
        """
    
        def div(self, a, b):
    				#함수에 대한 설명 
    
            """Return quotient of two given numbers.
    
            Given two numbers float type a, b
            returns float type a/b.
    
            Arg:
                a (float): The dividend of dvision function.
                b (float): The divisor of dvision function.
    
            Return:
                float: The quotient fo two given numbers.
            """
            return a / b

## Process4
function test 코드 만들기

    """Test cases for calculator.py."""
    
    from calculator import Calculator


​    
    class TestCalculator():
        """This class has 4 methods to test add, sub, mul, and div respectively.
    
        Note that the test class doesn't have a constructor.
    
        !!Don't forget to check all implementation covered by tests.
        """
    
        def test_add(self):
            """Put your test cases for add that starts with assert."""
            pass
    
        def test_sub(self):
            """Put your test cases for sub that starts with assert."""
            pass
    
        def test_mul(self):
            """Put your test cases for mul that starts with assert."""
            pass
    
        def test_div(self):
            """Put your test cases for div that starts with assert."""
            calc = Calculator()
            result = calc.div(4.2, 2.1)
            assert result == 2
    
            result = calc.div(5, 2.)
            assert result == 2.5
    
            result = calc.div(100, 20.)
            assert result == 5
    
            result = calc.div(5.e10, 10.)
            assert result == 5.e9
    
            result = calc.div(1., 1.e5)
            assert result == 1.e-5

## Process 5

Commit 하기 

    git commit -m "Practice commit and pull request"

## Process 6
push 하기

    git push origin usr/wonseokjung/bitwise_shift_right

## Process 7 

pull request

![](https://www.dropbox.com/s/ic0zdc64p2w3sp8/Screenshot%202019-01-12%2014.06.37.png?raw=1)

Reviewer ,  assigneers, labels 선택
