## 들어가며
&nbsp;&nbsp;&nbsp;&nbsp;시간 날 때마다 Kaggle을 둘러보며 친숙해지려고 하는데 언어의 장벽 때문인지 아직도 낯설다. 그러던 중, [시작부터 하라](https://brunch.co.kr/@minwoo/19)는 글을 봐서 competition에 참여할까...싶기도 한데 아직 뭘 어떻게 해야할 지 감이 잘 안와 방향을 찾기 위해 검색해봤다. 검색 결과, [Best Kaggle competitions for beginners](https://www.kaggle.com/getting-started/44088)에서 list up된 competition을 찾았다. analysis method에 따라 정리가 잘 되어 있는 것 같으니 적당한 competition을 찾아 시작해봐야겠다. 더불어 저번에 찾아둔 [Kaggle solution](http://kagglesolutions.com/?fbclid=IwAR025fM8UE00AzukabN19-CfWVcXTyMEXapyQ3IYmTyVJoPv-x5o51OuztA)를 보며 ranking 안에 들려면 어떤 식의 결과를 어떻게 내야 하는지 공부해볼 생각이다.  
&nbsp;&nbsp;&nbsp;&nbsp;잡설이 길었는데, 이 글은 Kaggle을 시작하고 싶지만 여전히 방향을 못잡은 나를 위한 글이다. '일단 notebook부터 만들어라'라고 할 수도 있는데, 난 대략적인 방향과 목적없이 무턱대고 시작부터 하는 걸 잘 못하는 편이라 뭔가를 시작할 땐 이렇게 좀 오래 걸리는 편이다. 시작이 오래걸린 만큼 꾸준히 할 수 있길 바라며. 


> 이 글은 [10 Tips to Get Started with Kaggle](https://opendatascience.com/10-tips-to-get-started-with-kaggle/)과 아마 저 글을 보고 번역한 많은 사람들의 글을 내 식대로 해석한 글이다. 따라서 내게 낯선 표현보다는 통계학을 공부하며 봤던 익숙한 표현들로 다시 해석하는 몇 개의 단어나 문장이 있을 것이다. 


# 1. Data science를 위한 프로그래밍 환경 정하자.
&nbsp;&nbsp;&nbsp;&nbsp;Data science를, 혹은 Data analysis를 할 수 있는 tool은 굉장히 많다. 대표적으로 들 수 있는 Python, R, SAS, SPSS, Matlab 말고도 내가 아는 것만 10개가 넘는다. 목적과 본인이 속한 환경에 따라 익숙한 tool이 있겠지만 Kaggle에선 Python과 R로 작성된 notebook이 대부분이고 엄밀히 말하면 Python으로 작성된 것이 R보다 훨씬 많다. 따라서 kaggle을 지금 막 시작하는 사람이 있다면 난 Python을 추천한다.
- Python과 R의 성능차이를 따져서 Python이 더 대중화된 건 아니라고 생각한다. 개인적으로 R은 통계 분석 tool에 국한된 느낌이라면 Python은 훨씬 넓은 범위에서 사용할 수 있기 때문에 Python이 Kaggle에서 높은 위상을 갖게 된 것이 아닌가 추측한다.  

# 2. 이미 많이 사용된 test dataset으로 연습하자.
&nbsp;&nbsp;&nbsp;&nbsp;code를 이용한 data analysis에서 실력을 가장 빠르게 키울 수 있는 방법은 raw dataset을 하나 잡고 실제로 분석을 해보는 것이다. 이 때, raw dataset을 선택할 때 이미 많이 분석된 dataset을 고르는 것이 좋다. 누군가 가르쳐주는 것이 아닌 self study로 data analysis를 시작하는 경우 자신이 낸 결과와 **비교해볼 다른 사람의 결과**가 꼭 필요하다. 비교해볼 다른 사람의 결과는 beginner들에게 답안지와 같다. 때문에 beginner들의 답안지가 차고 넘치는 titanic dataset을 많은 사람들이 Kaggle beginner에게 를 추천하는 이유다. 
&nbsp;&nbsp;&nbsp;&nbsp;Kaggle에서 제공하는 raw dataset이 대부분 그렇지만 kaggle에서 많이 사용된 test dataset은 실제로 data analysis의 대상이 되는 raw dataset과 유사한 형태다. 유사한 형태라함은 raw dataset이라는 말 그대로 '날 것' 그 자체여서 가공없이는 분석이 어려운 형태를 의미한다. data analysis의 대부분이 data handling이라는 것이 이미 많이 알려져 있으므로 이를 연습하려면 'raw dataset'이 최고다. 
- raw dataset을 구할 수 있는 웹사이트는 생각보다 많다. Kaggle에서도 구할 수 있고 [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets.html)와 같은 Repository, 혹은 Data Camp처럼 data analysis를 배울 수 있는 사이트에서도 raw dataset에 대한 소스를 얻을 수 있다. 
- 물론 이렇게 돌아다니는 raw dataset은 '대용량'은 아니다. 근데 몇 기가, 몇 테라짜리 dataset이나 몇 메가 짜리 dataset이나 algorithm은 똑같으니 섭섭해 하지 말자. 

# 3. data를 레고처럼 여러 형태로 만들어 갖고 놀자. (Preprocessing 1/2)
&nbsp;&nbsp;&nbsp;&nbsp;Data transformation, 그러니까 data wrangling, data munging, data handling 등등, 용어와 쓰임이 명확하게 정의되지 않았지만 결론은 하나다. **data의 형태를 목적에 따라 바꾸는 것.** Preprocessing 즉 전처리 과정을 두 개로 나눈다면 아마 이 과정이 첫 번째일 것이다. 여러 개의 dataset를 합치고 나누고 조건에따라 거르기도 하고 버리기도 하며 outliers와 missing values를 찾고 어떻게 다룰 것인지 결정하는 일련의 모든 과정이 여기에 속한다. data transformation은 상당히 긴 시간을 잡아먹는 과정이므로 능숙하게 할 수 있도록 연습해야 한다. 

# 4. Preprocessing의 꽃은 변수 조작이다. (Preprocessing 2/2)
&nbsp;&nbsp;&nbsp;&nbsp;Kaggle competition의 입상자들과 입상하지 못한 다른 사람들의 방법론적 차이는 거의 없다. 그도 그럴게, 입상 여부와 관계없이 보통 Python에 이미 존재하는 model을 사용하기 때문에 model이나 algorithm에서 오는 차이는 별로 없는게 맞다. 차이는 **변수 조작**에서 온다. data analysis에서 변수 조작은 매우 중요하다. 변수 조작은 model fitting 전의 **변수 변환**과 model fitting 후의 **변수 선택**으로 나뉜다. 분석의 목적과 변수의 종류에 따라 쓸 수 있는 model이 달라지고 model에 변수의 개수도 model fitting에 영향을 미친다. 따라서 model에 쓸 수 있는 상태의 dataset을 만들었다면 변수 변환을 해야한다. 변수 변환이란 변수들이 갖고 있는 정보를 최대한 놓치지 않은 선에서 변수를 합치거나 쓸모없는 변수를 버리거나 하는 등의 작업이다. 변수 선택은 model fitting 후 model에 속한 변수를 솎아주는 작업이다. 이 두 과정을 거치면 더 좋은 결과를 만들 수 있을 것이다. 

- 3.과 4.를 합쳐서 data '블라블라'(요즘은 data wrangling 이나 data munging으로 말하는 추세인 듯 하다.)로 말하는 경우도 있고 아닌 경우도 있고 각자의 맘이다. 분리해서 보는 경우가 더 많은 것 같기는 하지만 이게 학계에서 합의된 사항인지는 잘 모르겠다. 사실 변수 조작이라고 번역하는 것도 좀 이상하긴 한데 마땅한 단어가 생각이 안난다. 

- Feature engineering를 나는 **변수 조작**이라 번역했다. 이유는 딴 게 없고 kaggle에서 많이 보이는 feature라는 말이 dataset의 column을 의미하고 이를 '변수'로 더 많이 불렀기 때문이다. engineering인 이유는 이 과정이 알고리즘에 근거한 수치에 근거하는 부분이 커서 공학적인 느낌이 있어서가 아닐까? 

# 5. Ensemble method를 어떻게 써야하는지 배우자.
&nbsp;&nbsp;&nbsp;&nbsp;Ensemble learning 혹은 Ensemble method는 머신러닝에서 여러개의 모델을 학습시켜 그 모델들의 예측결과들을 이용해 하나의 모델보다 더 나은 값을 예측하는 방법이다. 많이 쓰이는 Ensemble method는 Random forest, Bagging, Boosting, Stacking 등이 있다. 이 방법들을 사용해 kaggle에서 우승한 사례가 많다고 하고 실제로 learning에서 많이 사용하기도 하니 익혀두면 두고두고 쓸데가 있을 것이다.

# 6. overfitting을 어떻게 해결해야 하는지 배우자.
&nbsp;&nbsp;&nbsp;&nbsp;overfitting이란 training dataset에서는 well-prediction model이 testing dataset에서 bad-prediction model이 되는, 즉 model이 training dataset에 '과적합'되는 현상을 말한다. train, test dataset으로 나누는 learning method에서 overfitting은 어쩌면 필연적일지 모르겠다. 그만큼 자주(어쩌면 항상) 발생하는 문제이므로 반드시 해결 방법에 대해 알아두어야 한다. 다행인 것은 이미 많은 overfitting 완화 방법들이 Python 내에서 제공되고 있으므로 어떻게 대처해야 하는지 꼭 배워두자. 

- Python에서 방법들을 제공한다고 했지 그 방법들로 overfitting을 해결할 수 있다고는 안했다. overfitting는 learning분야에서 활발하게 연구되고 있는 분야다. 

# 7. Forum을 사용하자. 
&nbsp;&nbsp;&nbsp;&nbsp;대부분의 coding 작업이 그렇듯 혼자서 한 번에 뚝딱뚝딱 완성하기는 매우 힘들다. 더욱이 data analysis는 조건만 갖추면 function이 돌아가버리니 이게 맞는지 틀린지 제대로 됐는지 안 됐는지 확인할 방법이 없다. 다행히 이미 많은 data analysis 관련 forum들이 세계적으로 존재하고 한국에도 몇 개 있어 통계적 방법론이나 코딩에 대해 묻고 답을 얻을 수 있다. 뿐만 아니라 해당 분야에 대한 insight도 넓힐 수 있으니 kaggle beginner들은 꼭 2-3개 씩의 포럼을 즐겨찾기 해놓자. 
- 한국의 경우 facebook page가 활성화 되어 있다. 대표적으로 통계에 대한 대부분의 것을 주제로 하는 [통계마당](https://www.facebook.com/groups/632755063474501/?hc_ref=ARS8oVhiEzHntxlb2eWsiIRlS_6_losEDe9w8R_jFsx9zSGdcXg6yNm_5rAhLsQNlAs&__tn__=C-R), Python programing 중심의 [Python Korea](https://www.facebook.com/groups/pythonkorea/?ref=nf_target&fref=nf), kaggle 중심의 [캐글 코리아](https://www.facebook.com/groups/KaggleKoreaOpenGroup/?hc_ref=ARRmYPL19nJaYyOPDXqIH54nx6KPvUaVW6P8DhF3QwR9ct7_h3n9u8E1fxsjI3xJpRk) 정도가 있다.

# 8. 자신만의 kaggle toolbox를 만들자. 
&nbsp;&nbsp;&nbsp;&nbsp;dataset을 이용해 data analysis를 진행하면 고정적으로 진행하는 작업들이 있기 마련이다. (가령 .info()를 확인한다던가 하는 것들) 이 작업들을 사용자 정의 함수로 묶어서 자신만의 함수를 만들어보는 건 coding 실력의 향상 뿐 아니라 work flow를 익힐 수 있는 좋은 기회일 것이다. 더 나아가 load부터 transform, model fitting까지의 과정을 data pipeline으로 구축한다면 오타로 인한 실수는 줄이고 작업 효율은 늘릴 수 있는 일석이조의 경험이 될 것이다. 

# 9. 과거의 kaggle competition으로 연습하라.
&nbsp;&nbsp;&nbsp;&nbsp;기출문제를 푸는 공부 방법은 시험을 준비하는 데 있어서 정말 좋은 방법이다. 물론 Kaggle competition에 완전히 같은 문제가 나오진 않겠지만 data analysis의 work-flow는 비슷비슷하다. 즉, 과거의 competition으로 연습하는 것은 기출문제로 문제 해결의 과정을 연습하는 것과 같다. 이 방법의 더욱 좋은 점은 상위 순위에 rank되어 있는 참가자들의 해결 방법들을 볼 수 있다는 데에 있다. 그들의 방법을 베끼는 것이 아니라 **어떤 문제에서 어떤 방법론을 왜, 어떻게 사용했는지** 궁리하며 공부해야 한다. 보고 생각하고 commit해라. 그러다보면 언젠가 상위 10% 안에 들 수 있을 것이다. 
- 10 Tips to Get Started with Kaggle에서 추천하는 링크, [Seven Python Kernels from Kaggle You Need to See Right Now](https://opendatascience.com/seven-python-kernels-from-kaggle-you-need-to-see-right-now/), 다. 
- 생각해보니 10 Tips to Get Started with Kaggle가 적힌 [opendatascience](https://opendatascience.com/)도 매우 좋은 사이트가 같다. 저장해야지. 

# 10. 지금 바로 시작하기.
&nbsp;&nbsp;&nbsp;&nbsp;대부분의 자기개발서 등의, Tip을 논하는 글이나 책의 항상 마지막은 이런 식이지만 틀린 말은 아니다. '공부'만 하려고 kaggle을 시작한 사람은 kaggle을 반만 이용하는 것이다. kaggle은 수 많은 경연이 열리는 공간이다. 경연의 상품은 당연히 돈이다! (심지어 생각보다 많다.) 그러니 이제 시작하자. 맘에 드는 dataset이든, 맘에 드는 회사의 competition이든 그것도 아니라면 그냥 상금이 많든, 일단 참가부터 하자. 참가하고 dataset 받고 dataset 앞에서 고민해야 성과가 나온다. 
