# Learned
crwaling, preprocessing/Classiifcation, Visualization

> 20.12 ~ 21.03 배운 점

1. Crwaling 
 * 공공데이터 포털, 통계청 자료 수집
  + selenium, 자동 정보 수집
   # pip, Selenium, Webdriver, xmlpath, selector, API
    - explain
      가용한 파일을 확인하기 위해 공공데이터 포털의 리스트(파일명, 제공형태, 설명 등)를 긁어와야했다. BeautifulSoup을 활용해 데이터 크롤링 결과 원하던 데이터를 얻을 수 없었고,
     그 결과 찾은게 selenium이였다. Selenium은 요소에 대해 click, enter 등의 행위를 수행한다는 점이 BeautifulSoup와 차이점이였으며 그결과 데이터 파일 목록을 무사히 수집할 수 있었다.
    
     * Selenium을 사용해 공공데이터 포털의 데이터를 추출했으나, pagination은 구현이 어려워 url의 page번호를 교체하는 방식으로 대체했다. (추후 확인 필요)
     * 통계청의 경우 상위 폴더 click은 가능하나, 하위 항목을 click하는 방법을 구현하지 못 해 각각 수작업으로 선택한 뒤 내용을 가져왔다.
     (21.02.25 기준 web ui변경되었다.)
     * 특정 분야에 대한 '공공데이터 파일 목록' 리스트와 '통계청 목록' 리스트 생성.
     
2. Preprocessing / Classification
 * 수집한 데이터 전처리
  + re, 패턴 찾기
   # re, KoNLP
    - explain
     수집한 데이터 중 '공공데이터 파일 목록'의 raw 데이터는 ['제목, 제공기관, 설명, 파일형태, 제공형태, 수정일']이였고, 목적에 맞는 데이터를 찾기 위해서는 분류가 필요했다.
    첫 분류시 보건복지부 정책 기준으로 분류했으나, 여러 곳에서 섞인 데이터를 복지부 조직도 기준으로만 나누기는 한계가 있다고 판단해 보건복지부 조직도 기준으로 분류하였고
    재 분류 하였으며 이 과정에서 ['부서','하위부서','대분류','중분류','소분류','요약'] 컬럼을 추가로 생성했다.
    
    * compare, 공공데이터 파일 수집후 새로운 데이터(ex, 수정일자 변경, 새로운 데이터 추가 등)생성시 이전 데이터와 비교해 차이가 있는 자료만 업데이트 수행했다.
    
    * 요약 컬럼대신 태그 컬럼을 처음 만들었으며, 이를 분류중 KoNLP 패키지를 사용해 명사들을 분리했으나 키워드 또는 피벗시 간단하게 알 수 있는 컬럼이 좋겠다는 의견을 반영해
      단어 부서 또는 지역 분류후 뒷 부분을 요약 컬럼으로 생성했다.

(번외) PDF to Excel
 * PDF파일 엑셀로 변환
  + pdftables, PyPDF2, pdfminer
   # pdftables API
    - explain 
     PDF를 엑셀로 변환해야할 상황이 생겼다. 약 3-400 페이지에 달하는 PDF는 알집/ConverPDF(사이트) 변환할 경우 각주 부분이 다른 셀로 이동되거나 수식이 동일 row의 여러 셀에 흩뿌려진 
     걸 볼 수 있었다. 표의 경우 데이터 나누는 줄이 없는 경우 한 셀에 여러개의 숫자가 입력되어 있었기에 변환이 필요했고 그렇게 변환한 페이지를 sheet별로 입력해야했다.
      googling 결과 pdf내용을 읽어오는 데는 성공했으나 줄글로 읽어오기에 sheet별로 삽입하는것이 어렵다 판단되어 pdftables의 api key를 받아 변환 작업을 수행했다.
   
  
  
3. Visualization
  * 국민건강보험공단 진료내역정보.csv
    + colab, jupyter notebook
     # Colab, Foilum, seaborn, matplotlib, groupby, agg
      - explain
       진료내역 정보는 총 3개의 파일로 100만명의 진료내역이 들어있는 데이터이다. 각 파일은 35만, 35만, 30만명의 진료 데이터를 갖으며, 동일인이 여러번 진료를 받은 기록까지 포함한다면 
       약 462만개의 데이터를 갖는 셈이다. 이를 PC에서 돌릴 경우 데이터를 읽는데만도 시간이 소요되기에 비효율적으로 생각되 notebook을 사용하게 되었다. 
        기존에는 jupyter notebook을 사용했으나, 공유 및 마크다운 특히 목차가 제공된다는 점으로 인해 google colab을 사용하게 되었다.
       
       * 시각화에 대한 검색 중 지도에 자료를 표시할 수 있는 패키지 Folium을 알게 되었고 마침 데이터 중 지역정보(시도코드)를 가진 데이터가 있기에 활용해봤으며,
         지역별 진료자 찾기의 경우 대부분 수도권에 몰려있어 다른 지역의 확연한 차이는 볼 수 없었으나 이는 첫번째 데이터만 활용한 경우이고 100만명 모두 사용하면 다른 결과가 도출 되지          않을까 하는 생각이다.
