64bit(rdi,rsi,rdx,rcx)

ctors, dtros 영역
 - main 앞뒤에 실행하는 함수(?)
 - readelf -s

e=ELF('...실행파일...')
 - e.got[printf] -> printf의 got주소

scanf - 공백까지만 입력받음
gets - 개행문자까지만 읽고 개행문자는 버린다
fgets - 개행문자도 포함해서 읽어온다.
 
gmpy2
 - iroot -> 제곱근
 - gcd
 - powmod
 - invert

nextprime() 다음 소수 찾아줌 

32bit fsb에선 stack이 바로 출력, address에 \x00이 안들어가서 ㄱㅊ
64bit에선 RSI RDX RCX R8 R9 stack순으로 출력, address에 \x00이 들어가서 printf할때 짤림, 따라서 %c를 제일앞에 두고 address를 제일 뒤로 넘긴다.

e=elf('파일명')
e.got['함수'], e.plt['함수'] -> got, plt address
libc=e.libc
  libc.symbols['함수명'] -> offset

 peda에서 elfsymbol 함수명 ->plt, got 다 나옴

syscall
 - eax의 값에 따라 syscall함수를 호출함, 인자는 ebx,ecx,edx...로 들어감, 마지막엔 int 0x80을 호출해줘야함(interrupt)

exp.py 작성시 꿀팁
	if __debug__:
		local
	else:
		remote
	python 에서 옵션 -O주면 remote실행, 안주면 local실행

recv,send할때 sync를 잘생각해야함, sync를 못 맞춘다면 buffer의 크기를 고려해서 payload를 작성

return to csu
 csu에 있는 gadget을 사용함
 pop rbx, rbp, r12, r13, r14, r15, ret 이 있고(+90)
 (+27)위에서 받은 값들을 rdx, rsi, edi에 입력하는 가젯이 있다.
 rbx=0, rbp=1, r12=함수got, r13=rdx r14=rsi, r15=edi순으로 들어간다.(os마다 약간씩 차이는 있는듯 하다)

/proc/self(or pid)/fd ->fd info

libc 찾기
	실제 주소 찾아서 하위3byte로 libc-database에서 library검색
	해당 library(libc-database/db/)로 offset등 검색가능
	one_gadget도 돌려서 찾을 수 있음
	