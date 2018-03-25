Preference Shooting(취향저격프로젝트)
===================
사람의 취향은 선호하는 예술작품을 통해 드러날 수 있다. 어떤 것이 아름답다고 느끼고 끌리는지는 작품의 선호도를 통해 드러나고, 이러한 취향은 브랜드를 선택하는 데 영향을 미칠 것이라고 생각했다. 따라서, 본 프로젝트에서는 작품의 선호도와 브랜드 선호도를 분석하여 둘의 상관관계를 파악하고,이 정보를 통해 회사들은 자신의 브랜드를 좋아하는 사람들에 맞춘 마케팅 전략을 세울 수 있다.
<p align="center">
  프로젝트에 대한 설명은 아래 YouTube에서 간단하게 확인할 수 있다.
  <a href="https://www.youtube.com/watch?v=rPOJF1KttCI"><img src="/screenshots/prefer-video.png" width="60%"></img></a>  
</p>

#### Implementation
- Language: Python 2.7
- Tool: ipython notebook, visual studio code, sublime text
- Version control tool: Github
- Database: IMDB(6099 movies), Apparel search(342 brands)
- API: Instagram API, The Echo nest(3256 musics)
- Data Learning model: [SVM](http://scikit-learn.org/stable/modules/svm.html)
******
## Process
1. Construct DataBase
2. Search Instagram user ID including movie tags
3. Search brand name and music tags for each user id
4. Train the data with SVM model
5. Make application to evaluate the model

## 1. Construct Movie DB, Music DB, and Brand DB
우리는 영화와 음악 그리고 브랜드 데이터를 수집하였다. 
영화는 [iMDB](http://www.imdb.com/)에서 1992년부터 2014년까지의 영화 68,316개를 직접 crawling하였고, 각 영화마다 7개의 feature(장르, 감독, 평점, 평점개수, 제목, 출연자, 연도)를 모았다.
우리가 원하는 음악 데이터를 모으기 위해 여러 사이트와 api를 찾아다닌 결과, [The Echo Nest](http://static.echonest.com/enspex/)에서는 꽤나 큰 음악 database를 가지고 있었고, 그 중 우리가 필요로 했던 여러 feature 정보도 포함하고 있었다. 본 프로젝트에서는 The Echo Nest api를 통해 음악 제목, id, 아티스트, enery, liveness, tempo, speechiness, acousticness, danceability, instrumentalness, loudness, valeance, song hotttness, song type, artist terms 정보를 포함한 음악 3,256개를 모았다.
마지막으로 apparel search 웹사이트를 통해 342개의 apparel brand 리스트를 얻었다.
'''
> Movie/parser.py -> iMDB 사이트를 파싱하여 Movie_DB.txt를 만든다.
> Music/get_music_data.py -> echo nest api를 이용해 Music_DB.txt를 만든다.
> Brand/brandlist.py -> brand_DB.txt를 만든다.
아래 screenshot은 우리가 모은 영화와 음악 데이터셋의 일부이다.
<p align="center">
 <img src="screenshots/prefer-movie.png" width="40%"></img> <img src="screenshots/prefer-music.png" width="40%"></img>
</p>

## 2. Search Instagram user ID including movie or music tags
우리는 어떤 한 사람이 좋아하는 영화나 음악 정보를 가지고 그 사람이 선호할만한 브랜드를 추천해줄 것이다. 
그러기 위해서는 training set를 만들어야 하는데, 우리는 instagram의 해시태그 정보를 활용하였다.
구현 과정을 설명하면, 우리가 모은 movieDB에 있는 영화 제목으로 인스타그램 태그를 검색하여 가장 많은 게시글이 달린 태그를 받아와 그 태그가 달린 게시글을 쓴 유저들을 받아온다. 
이 때 영화제목으로 가능한 검색 조합을 생성하여 검색한다. 
'''
> 예를 들어 영화제목이 "Harry Potter and the Deathly Hallows: Part 2" 라면, 
>  >생성 가능한 검색어는 "harry potter", "harry potter and the deathly hallows", "the deathly hallows", "Harry Potter and the Deathly Hallows: Part 2" 등이 될 수 있다.
이렇게 검색한 영화들로 이 영화를 태그한 사용자 정보를 모아 /instagram/user_movie_DB.txt로 만들었다.

