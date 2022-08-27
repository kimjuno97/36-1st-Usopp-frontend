   # 1. 팀 소개
   > 프로젝트 기간
   > 2022.08.12 ~ 2022.8.26 <br>


   Usopp

   |포지션|이름|담당|
   |---|---|---|
   |FRONT|김준호|로그인, footer|
   |FRONT|소재현|nav bar, 제품상세페이지|
   |FRONT|이빛나| 메인페이지, 제품상세페이지|
   |BACK|류예린|장바구니 CRUD 구현.|
   |BACK|이상우|로그인 & 회원가입 구현.|
   |BACK|이상엽|사이트 페이지 구현.|

# <br />





## :: 구현 목표

- 이솝 홈페이지 usefarams, map함수, props를 사용하여 UI구현 
- 로그인 입력시 유효성 검사 함수에 따른 화면 구현 
- 이메일 정보를 호출하여 DB에 없다면 메세지 출력과 회원가입으로 이동.
- 이메일 정보 일치시 비밀번호 입력란 호버 출력.
- 정규집에 맞는 아이디 및 비밀번호 일치시 게정 생성.
- 메인 페이지 및 카테고리별, 상품별 페이지 구현.
- 장바구니에 상품 담기, 삭제, 수량조절 구현.

<br />

## :: 구현 사항 설명

1. 로그인 입력 데이터 유효성 검사 (프론트)

- 입력창에 변화가 일어날 때 마다 유효성 검사 함수가 실행된다.
- 유효성 검사 함수의 반환값(boolean)에 따라 버튼의 배경색과 disabled 속성이 변화한다.
- 테스트 방법 : `/login` 페이지 이동 >>> 로그인 input 데이터 입력

2. 장바구니 구현 (프론트)

- 장바구니 추가 버튼 클릭시 데이터를 백엔드로 전달해서 장바구니로 다시전달 

2. 로그인 데이터 처리 (백엔드)

- 조건문과 정규식을 이용한 email과 password 유효성 검사를 진행
- 유효섬 검사를 통과하지 못 할 경우 에러 메시지를 반환 예외 처리 구현

3. 제품 페이지 구현 (백엔드)
- 메인 페이지 단품 및 그룹 상품 동시 출력.
- 카테고리별 상품 출력 구현.
- 상품별 상세 페이지 구현.

4. 장바구니 CRUD (백엔드)
- 장바구니 생성 구현.
- 장바구니 상품 조회 구현.
- 장바구니 상품 개별 및 전체 삭제 구현.
- 장바구니내의 수량 조절 구현.

<br />

## :: 성장 포인트 

- 프론트엔드와 백엔드간의 소통의 중요성.
- 프론트엔드와 백엔드 각 프로젝트 플로우 인지.
- 각 분야별 DB처리 방식 및 통신 오류 요소 및 해결.
- 
<br />






</aside>

# ::  프로젝트 맡은 부분 상세 구현 설명 




<aside>

## 1. login

### :: 구현 사항 설명

 - 로그인 과정 
 
   - 처음 로그인창에는 email input창 하나만 있으며, email을 서버에 전송합니다. (프론트엔드)
  
    - 서베에 전송된 email은 DB에 등록된 회원인지 아닌지 판별후 메세지를 보냅니다. (백엔드)
  
      - 등록된회원
         - 등록된 회원이면, 서버에서 로그인 요청을 받게되고, 그 요청을 받으면 password 입력창이 추가로 나타납니다.
         
         - 그리고 입력된 email, password 을 서버에 전송합니다.
         
         - 서버에서 회원정보가 일치하면 토큰을 보내고, 이를 받아 localstorage에 저장합니다.
         
       - 회원가입
       
         - 등록되지않은 회원이면 서버에서 회원가입 요청을 받게되며, 회원가입에 필요한 입력창이 추가로 나타나고 회원가입을 진행합니다.
         
         - 회원가입 정보를 서버에 전송이 성공하면, 다시 처음 로그인창으로 돌아갑니다.
         
         - 이때 전송한 데이터가 이미 가입된 회원이거나, 조건을 만족하지 못하면 회원가입이 되지 않습니다.



 ### :: 코드 
 
 로그인 창 UI는 상황에 따라 3가지 경우가 있습니다.
우선 이를 상수데이터로 필요한 정보를 정리 했습니다.


                const SIGN = {
                  sign: [{ text: '이메일 주소', name: 'email' }],
                  signIn: [
                    { text: '이메일 주소', name: 'email' },
                    { text: '패스워드', name: 'password' },
                  ],
                  signUp: [
                    { text: '이메일 주소', name: 'email' },
                    { text: '패스워드', name: 'password' },
                    { text: '패스워드 확인', name: 'passwordConfirm' },
                  ],
                };




그 후 signMode라는 useState를 생성해 조건에 따라 보여지는 UI를 다르게 했습니다.


    const [signMode, setSignMode] = useState('sign');


       const showSign = {
          sign: (
            <SignIn
              sign={SIGN.sign}
              inputValue={inputValue}
              saveInput={saveInput}
              goToSignIn={confirm}
              signDisabled={signDisabled}
              signBtnColor={signBtnColor}
            />
          ),
          signIn: (
            <SignIn
              sign={SIGN.signIn}
              inputValue={inputValue}
              saveInput={saveInput}
              goToSignIn={goToSignIn}
              signDisabled={signDisabled}
              signBtnColor={signBtnColor}
              signIn={signIn}
            />
          ),
          signUp: (
            <SignUp
              sign={SIGN.signUp}
              inputValue={inputValue}
              saveInput={saveInput}
              checkBox={checkBox}
              checked={checked}
              goToSignUP={goToSignUP}
              signUpDisabled={signUpDisabled}
              signUpBtnColor={signUpBtnColor}
              signIn={signIn}
            />
          ),
        };
        

       // return  아래
         
         {showSign[signMode]}



여기서 컴포넌트는 SignIn, SignUp 두가지가 있습니다. 
이메일 창만 있을때와 로그인 창은 input 창 하나 있고 없고 차이입니다.

그렇게 때문에 SignIn에서 props로 받는 데이터도 한개 차이가 나는데, 그 데이터의 유무에 따른 조건부 렌더링을 사용 했습니다.


     const SignIn = ({
       sign,
       inputValue,
       saveInput,
       goToSignIn,
       signDisabled,
       signBtnColor,
       signIn,    // signIn이 있으면 로그인창 없으면, email창 1개
     }) => {
       return (
         <>
           <Input sign={sign} inputValue={inputValue} saveInput={saveInput} />
           <button
             className="btn"
             onClick={goToSignIn}
             disabled={!signDisabled}
             style={{ backgroundColor: signBtnColor }}
           >
             <span> 계속</span>
           </button>

           {signIn && (
             <p className="under" onClick={signIn}>
               회원이 아니십니까?
             </p>
           )}
         </>
       );
     };


 
 
 회원가입 UI는 css 부분에서 구조가 달라서 따로 컴포넌트를 만들었습니다.
 
 
       const SignUp = ({
        sign,
        inputValue,
        saveInput,
        checkBox,
        checked,
        goToSignUP,
        signUpDisabled,
        signUpBtnColor,
        signIn,
      }) => {
        return (
          <>
            <Input sign={sign} inputValue={inputValue} saveInput={saveInput} />
            <div className="nameInput">
              <input
                placeholder="성"
                name="firstName"
                value={inputValue.firstName}
                onChange={saveInput}
              />
              <input
                placeholder="이름"
                name="lastName"
                value={inputValue.lastName}
                onChange={saveInput}
              />
            </div>
            <div className="checkBox">
              <input
                type="checkbox"
                name="first"
                checked={checkBox.first}
                onChange={checked}
              />
              <span>가입자 본인은 만 14세 이상입니다.</span>
            </div>
            <div className="checkBox">
              <input
                type="checkbox"
                name="second"
                checked={checkBox.second}
                onChange={checked}
              />
              <span className="terms">이용 약관에 동의합니다.</span>
            </div>
            <button
              className="btn"
              onClick={goToSignUP}
              disabled={!signUpDisabled}
              style={{ backgroundColor: signUpBtnColor }}
            >
              <span> 등록</span>
            </button>
            <p className="under" onClick={signIn}>
              이솝 계정을 가지고 계십니까?
            </p>
          </>
        );
      };
 
 
 
 SignIn, SignUp 안에서도 구조가 같은 Input부분은 컴포넌트화 하여 재사용 했습니다.
여기서도 input type을 password로 설정해야 할때를 생각하여 type 부분을 변수로 지정했습니다.


      const Input = ({ sign, inputValue, saveInput }) => {
        return (
          <>
            {sign.map(({ text, name }, idx) => {
              const type = text.includes('패스워드') ? 'password' : 'text';
              return (
                <div className="emailInput" key={idx}>
                  <input
                    type={type}
                    placeholder={text}
                    name={name}
                    value={inputValue.text}
                    onChange={saveInput}
                  />
                </div>
              );
            })}
          </>
        );
      };

 
 ### :: 성장포인트
 
 - 상황에 따른 컴포넌트 어떻게 더 재사용 할 수 있을까 고민을 하고 줄여 보았다.
 - 그 과정에서 props로 넘겨 받은 후 변수 값의 유뮤에 따라  조건부 렌더링을 할 수 있게 되었다.
 
## 2. 장바구니 

### :: 구현 사항 설명

 -  장바구니 버튼을 누르면 DB에 저장되어 있는 장바구니 리스트 목록을 fetch메서드로 받아서 useState로 관리합니다.
장바구니에서 일어나는 수량의 변화 또는 목록의 삭제는 'productData'를 업데이트하여 관리합니다.


          const [productData, setProductData] = useState([]);

          useEffect(() => {
            fetch('api주소')
              .then(response => response.json())
              .then(setProductData);
          }, []);


- 우선 상품 리스트별로 컴포넌트화 합니다.
컴포넌트화 하는 과정에서 상품의 총 합을 계산하기위해 , return위에서 map으로 컴포넌트를 만들면서 총 합계를 계산합니다.

      let totalSumPrice = 0;

       const productList = productData.map((product, idx) => {
         totalSumPrice += product.price * product.count;
         return (
           <Product
             key={idx}
             idx={idx}
             product={product}
             deletedList={deletedList}
             productData={productData}
             setProductData={setProductData}
           />
         );
       });


- Product 컴포넌트 내부입니다

   - idx는 인덱스번호이며 productData를 업데이트 할때마다 몇번째 인덱스를 변경해야할지 알아야하기 때문에 필요합니다.
   
   - 'showCountList' 함수 내부를 보면 함수형 업데이트를 사용했는데, 이는 업데이트 할 때 전 상태를 정확히 특정 짓기 위해 사용했습니다.
   
   - 조건부 랜더링을 활용하여 '삭제 버튼', '수량을 바꾸는 list' 를 활상화 비활상화 했습니다.


            const Product = ({
               idx,  // 수정해야할 인덱스 번호를 알아야함
               product,
               deletedList,
               setProductData,
               productData,
             }) => {
               // 제품 각각의 정보가 있는  proudct는 직관성을 위해 컴포넌트 내부에서 구조 분해 할당을 하였다.
               const { id, name, size, count, price } = product;  

               const sumPrice = price * count;

               const [showBtn, setShowBtn] = useState(false);
               const [showCount, setShowCount] = useState(true);

               const showDelete = () => setShowBtn(true);

               const hideDelte = () => setShowBtn(false);

               // true, false만 바꾸는 함수지만, 전 상태를 뒤집는 함수이기 때문에 확실한 전 상태를 뒤집기 위해 함수형 업데이트로 표현
               const showCountList = e => {
                 setShowCount(showCount => !showCount);
               };

               const changeCount = e => {
                 const newList = productData;
                 newList[idx].count = Number(e.target.innerText);  // props로 받아온 인덱스번호 활용 부분
                 setProductData([...newList]);
                 setShowCount(true);
                 fetch(`API주소`, {
                     method: 'PATCH',
                     headers: {
                       'Content-Type': 'application/json',
                       Authorization: localStorage.getItem('Token'),
                     },
                     body: JSON.stringify({
                       quantity: Number(e.target.innerText),
                       productId: product_id,
                     }),
                   });
               };

               return (
                 <div className="productLocation" key={key}>
                   <p className="titleArea">{name}</p>
                   <p className="sizeArea productArea">{size}</p>
                   <div
                     className="countArea btnLocation"
                     onMouseOver={showDelete}
                     onMouseLeave={hideDelte}
                   >
                     {showCount ? (
                       <>
                         <button className="quantity" type="number" onClick={showCountList}>
                           <p className="btnCount">{count}</p>
                           <i className="fi fi-rr-angle-small-down" />
                         </button>
                         {showBtn && (
                           <p className="deleteBtn" id={id} onClick={deletedList}>
                             삭제
                           </p>
                         )}
                       </>
                     ) : (
                       <ul
                         className="countList"
                         onClick={changeCount}
                         onMouseLeave={showCountList}
                       >
                         <li>1</li>
                         <li>2</li>
                         <li>3</li>
                         <li>4</li>
                         <li>5</li>
                       </ul>
                     )}
                   </div>
                   <p className="price">₩ {sumPrice.toLocaleString('ko-KR')}</p>
                 </div>
               );
             };


- 장바구니 리스트 삭제 구현

   - Product 컴포넌트에 전달한 product내부의 id 값을 삭제 버튼의 id로 주고, 삭제이벤트도 추가해 줍니다.
   
   - 삭제는 상위 컴포넌트에서 일어나야 productData를 업데이트 할 수 있습니다.


          // Product 컴포넌트(하위 컴포넌트)
         <p className="deleteBtn" id={id} onClick={deletedList}>
              삭제
         </p>

         // Cart 컴포넌트(상위 컴포넌트)
          const deletedList = e =>
            setProductData(
              productData.filter(product => product.id !== Number(e.target.id))
            );


- 수량  업데이트는 카트 내부에서 hook을 일으켜 렌더링을 해주고, fetch메서드로 수정했던 사항도 서버에 보내 줍니다.


         const changeCount = e => {
           const newList = productData;
           newList[idx].count = Number(e.target.innerText);
           setProductData([...newList]);
           setShowCount(true);
           fetch(`http://${API}:3000/carts`, {
            method: 'PATCH',
            headers: {
              'Content-Type': 'application/json',
              Authorization: localStorage.getItem('Token'),
            },
            body: JSON.stringify({
              quantity: Number(e.target.innerText),
              productId: product_id,
            }),
          });
         };


- . 결제하기를 누르면 장바구니 내부에서 업데이트 된 최신 상태의 productData를 전송합니다.


       const goToPay = () =>
         fetch(' http://518f-211-106-114-186.jp.ngrok.io/cart/2 ', {
            method: 'POST',
            headers: {
               'Content-Type': 'application/json',
               Authorization: localStorage.getItem('Token'),
            },
            body: JSON.stringify({
               userId: productData,
         }),
       })

 ### :: 성장포인트
 
 장바구니의 RUD 과정을 백엔드와 소통을 해보면서 구현 방법에 대한 고민을 많이 했었습니다.
 그 결과 삭제 업데이트 과정에서 백엔드에 해당 정보를 보내주고 UI적으로 업데이트는 state로 받아온 데이터를 재구성하는 방식으로 구현했습니다.
 쉽게말해 UD가 일어 날때마다 장바구니 API를 GET 하는게 아니라  GET은 장바구니가 열릴때, 한번만 요청하게끔, 구현했습니다.
 



## 3. Footer


### :: 구현 사항 설명

- display: grid로 레이아웃을 구현했습니다.

- 각 div 별로 gird-area로 구역명을 지정하고 grid-template-areas로 구역을 나누었다.


         //scss
         .footer {
            display: grid;
            grid-template-areas:
            'email email order service location'
            'vision vision introduce social caution';
            grid-column-gap: 40px;
            grid-row-gap: 40px;
            padding: 50px 40px;
         }


