# Vent Demo doc.

<br />

## 1. Concept

#### Old Server url : http://210.122.7.84:8080
#### Old Image base url : http://210.122.7.84:8080/static/img/
#### New Server url : http://nitingale.cloudapp.net:8080 [%HOST%]
#### Old Image base url : http://nitingale.cloudapp.net:8080/static/img/

#### Test GCM project id : TBA

----------
#### 모든 요청에 대한 응답은 json 형식으로만 지원됨.
#### 본 문서는 Github flavored markdown으로 작성되었습니다.
#### 정상적으로 문서를 보기 위해서는 해당 기능을 지원하는 Markdown 에디터나 Github gist에서 확인해주십시오. 
----------

### 1-0. API summary
#### 1-0-1. API list

0. [Dev](#dev)
	1. [GET - dev/swap_user/{id}(특정 id의 사용자로 전환)](#dev-swapuser)
1. [Auth](#auth)
	1. [POST - auth/signin(로그인)](#auth-signin)
	2. [POST - auth/signout(로그아웃)](#auth-signout)
	3. [POST - auth/signup(회원가입)](#auth-signup)
	4. [POST - auth/signin/simple(간편 로그인)](#auth-simple-signin)
	5. [POST - auth/signup/simple(간편 회원가입)](#auth-simple-signup)
	6. [PUT - auth/signup/simple(간편 회원 전환)](#auth-simple-update)
	4. [POST - auth/oauth/facebook(페이스북 웹 로그인, 회원가입 과정 포함)](#auth-oauth-facebook-web)
2. [User](#user)
	1. [GET - users/{id}(특정 사용자 정보)](#user-get-id)
	2. [GET- users/me(내 정보)](#user-get-id)
	3. [PUT- users/me(내 정보 수정)](#user-edit-me)
	3. [GET - users/{id}/articles(특정 사용자의 글)](#user-get-id-articles)
	4. [GET - users/me/articles(내 글 목록)](#user-get-id-articles)
	5. [GET - users/{id}/comments(특정 사용자의 댓글)](#user-get-id-comments)
	6. [GET - users/me/comments(내 댓글 목록)](#user-get-id-comments)
	7. [GET - users/{id}/activites(특정 사용자의 활동 내역)](#user-get-id-activities)
	8. [GET - users/me/activites(내 활동 내역)](#user-get-id-activities)
	9. [GET - users/me/subscribes(내가 구독하는 카테고리, 사용자의 게시물)](#user-subscribes)
	9. [GET - users/me/subscribes/categories(내가 구독하는 카테고리 목록)](#user-subscribes-categories-list)
	10. [POST - users/me/subscribes/categories/{category_id}(카테고리 구독)](#user-subscribe-category)
	11. [DELETE - users/me/subscribes/categories/{category_id}(카테고리 구독 취소)](#user-unsubscribe-category)
	10. [GET - users/me/subscribes/users(내가 구독하는 사용자 목록)](#user-subscribes-users)
	11. [GET - users/me/subscribes/categories/articles(내가 구독하는 카테고리의 게시물)](#user-subscribes-categories-articles)
	12. [GET - users/me/subscribes/users/articles(내가 구독하는 사용자의 게시물)](#user-subscribes-users-articles)
	10. [POST - users/{id}/subscribe(특정 사용자 구독)](#user-subscribe)
	10. [DELETE - users/{id}/subscribe(특정 사용자 구독 취소)](#user-unsubscribe)
	11. [POST - users/{id}/block(특정 사용자 차단)](#user-block)
	12. [DELETE - users/{id}/block(특정 사용자 차단 해제)](#user-unblock)
	13. [GET - users/{id}/comments/articles (특정 사용자 코멘트한 글 목록)] (#view_user_commented_articles)
	14. [GET - users/me/comments/articles (내가 코멘트한 글 목록)] (#view_me_commented_articles)
3. [Article](#article)
	1. [GET - articles/(최신순 글 목록)](#article-get)
	2. [GET - articles/{id}(특정 글 정보)](#article-get-id)
	3. [POST - articles/(글 등록)](#article-post)
	4. [DELETE - articles/{id}(글 삭제)](#article-delete)
	5. [GET - articles/{id}/comments(특정 글의 댓글 목록)](#article-get-id-comments)
	6. [POST - articles/{id}/comments(특정 글에 댓글 달기)](#article-post-id-comment)
	7. [POST - articles/{id}/like(특정 글에 좋아요)](#article-post-id-like)
	8. [DELETE - articles/{id}/like(특정 글에 좋아요 취소)](#article-delete-id-like)
	9. [GET - articles/categories/{id}(특정 카테고리 글 목록)](#article-get-category-list)
4. [Hashtag](#hashtag)
	1. [GET - hashtags/(다중 해시태그 검색)](#hashtag-get)
	1. [GET - hashtags/(다중 해시태그 검색)](#hashtag-get)
	1. [GET - hashtags/(다중 해시태그 검색)](#hashtag-get)
	2. [GET - hashtags/{tag}(특정 해시태그의 정보-사용된 글 목록)](#hashtag-get-tag)
	3. [GET - hashtags/search/{tag}(특정 해시태그와 비슷한 태그를 쓴 글 목록)](#hashtag-search-tag)
5. [Comment](#comment)
	1. [GET - comments/{id}(댓글 보기)](#comment-view)
	2. [DELETE - comments/{id}(댓글 삭제)](#comment-delete) 
	3. [POST - comments/{id}/like(댓글 좋아요)](#comment-like)
	4. [DELETE - comments/{id}/like(댓글 좋아요 취소)](#comment-dislike)

5. [Report](#report)
	1. [POST - reports/(신고)](#reports)
	2. [GET - reports/(신고목록)](#reports-list)
	3. [POST - reports/{id}(신고처리)](#reports-accept)
6. [Notification](#notification)
	1. [GET - notifications/(알림 내역)](#notification-list)


----------

----------

### 1-1. 페이징
#### 인자는 limit, page 두 가지로 페이징 기능을 이용 가능.
#### 페이징 인자가 없는 경우 기본값은 limit 5, page 1.
####### Example
######## http://nitingale.cloudapp.net:8080/articles 으로 GET 요청시
######## limit 인자가 5, page가 2인 경우, 6~10번째 값이 전달됨(offset, limit 기준 5,5가 됨)

----------

### 1.2 인증
#### -Session(Cookie) 기반 인증.

----------
### 1.3 응답 및 에러

#### 1.3.1 HTTP STATUS CODE
| Status | Description |
|---|---|
| 200 | OK |
| 201 | Created, Done |
| 400 | bad request |
| 401 | unauthorized(no permission) |
| 403 | forbidden |
| 404 | not found |
| 405 | unexpected method |
| 405 | internal server error |
| 500 | internal server error |

----------

#### 1.3.2 에러
##### 응답 status code가 200(OK), 201(Created)가 아닌 경우, result 값이 false가 됨. 이 경우 내부에서 Exception으로 처리되어 응답 값에 "type", "description", "code"가 들어가며, 에러 종류에 따라 추가 키가 들어감.
| Key | Description |
|---|---|
| type | 에러 식별을 위한 에러명 |
| description | 에러(Exception) 명 혹은 에러에 관한 추가 정보가 들어감. |
| code | Http status code |
| valid_methods | Method not allowed(잘못된 method로 요청한 경우, 사용가능한 메서드 목록이 들어감) |
| oauth_error_code | OAuth 인증에 실패한 경우 식별용 |
| info | 인자값이 잘못된 경우 이에 대한 추가 정보
	{
	    "code": 400,
	    "description": "Invalid arguments",
	    "info": {
	        "email": [
	            "This field is required."
	        ],
	        "password": [
	            "This field is required."
	        ]
	    },
	    "result": false,
	    "type": "InvalidArgument"
	}


#### HTTP STATUS CODE
| Status | Description |
|---|---|
| 200 | OK |
| 201 | Created, Done |
| 400 | bad request |
| 401 | unauthorized(no permission) |
| 403 | forbidden |
| 404 | not found |
| 405 | method not allowed |
| 500 | internal server error |
| 500 | permission denied |


<a name="Dev"></a>
## 0. Dev(테스트용 API)

<a name="dev-swapuser"></a>
### ``` GET ``` /dev/swap_user/{id}``` dev::swap_user```

###### Description

특정 사용자로 사용자 전환


###### URL Structure

`http://%HOST%/dev/swap_user/{id}`


###### Parameters

| Name | Required | Description |
|---|---|---|

###### Response


###### HTTP Status

* 200


###### Sample JSON Response
	{
    	"result": true
	}

###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="auth"></a>
## 1. Auth

<a name="auth-signin"></a>
### ``` POST ``` /auth/signin ``` auth::sign_in ```

###### Description

로그인


###### URL Structure

`http://%HOST%/auth/signin`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **email** | *Required* | email |
| **password** | *Required* | 원본(해시된 값 아님)|

###### Response


###### HTTP Status

* 200


###### Sample JSON Response
	{
    	"id": 3,
    	"result": true
	}

###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="auth-signout"></a>
### ``` POST ``` /auth/signout ``` auth::sign_out```

###### Description

로그아웃


###### URL Structure

`http://%HOST%/users/logout`


###### Parameters

| Name | Required | Description |
|---|---|---|



###### Response


###### HTTP Status

* 200


###### Sample JSON Response

	{
		"result": true
	}


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="auth-signup"></a>
### ``` POST ``` /auth/signup ``` auth::sign_up```

###### Description

회원가입


###### URL Structure

`http://%HOST%/auth/signup`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **email** | *Required* | email, 중복될 경우 에러 |
| **password** | *Required* | 원본(해시된 값 아님) |
| **nickname** | *Optional* | 닉네임(문자로 24바이트까지) |



###### Response


###### HTTP Status

* 200


###### Sample JSON Response

	{
		"result": true,
		"id": 3
	}


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="auth-simple-signup"></a>
### ``` POST ``` /auth/signup/simple ``` auth::simple_account_sign_up```

###### Description

간편회원가입
이메일이 임의로 생성되어 발급되며, 입력한 key로 로그인 가능

###### URL Structure

`http://%HOST%/auth/signup/simple`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **key** | *Required* | 고유값(IMEI 등) 중복될 경우 에러 |
| **nickname** | *Optional* | 닉네임(문자로 24바이트까지) |



###### Response


###### HTTP Status

* 200

###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="auth-simple-update"></a>
### ``` PUT ``` /auth/signup/simple ``` auth::simple_account_update```

###### Description

간편회원가입 계정에서 실제 이메일 주소로 재가입
이메일 인증 필요

###### URL Structure

`http://%HOST%/auth/signup/simple`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **key** | *Required* | 고유값(IMEI 등) 중복될 경우 에러 |
| **email** | *Required* | 이메일, 중복될 경우 에러 |
| **password** | *Required* | 비밀번호 |
| **nickname** | *Optional* | 닉네임(문자로 24바이트까지) |



###### Response


###### HTTP Status

* 200

###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />



<a name="auth-simple-signin"></a>
### ``` POST ``` /auth/signin/simple ``` auth::simple_account_sign_up```

###### Description

간편회원 로그인

###### URL Structure

`http://%HOST%/auth/signin/simple`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **key** | *Required* | 고유값(IMEI 등) 중복될 경우 에러 |


###### Response


###### HTTP Status

* 200

###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />

<a name="auth-forget-password"></a>
### ``` POST ``` /auth/forget ``` auth::forget_password```

###### Description

비밀번호 분실 시 임시 로그인 주소 이메일로 발송

###### URL Structure

`http://%HOST%/auth/forget`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **email** | *Required* | 이메일 미인증 또는 해당 정보 없을 시 에러 |



###### Response


###### HTTP Status

* 200


<br /><br />


<a name="auth-oauth-facebook-web"></a>
### ``` GET ``` /auth/oauth/facebook ``` auth::facebook_oauth_request```

###### Description

페이스북 웹 OAuth 인증 시작.
요청 후 응답값으로 오는 redirect 헤더를 따라 웹 연결 후 인증 진행.
기존에 사용한 적이 없는 사용자의 경우, 페이스북 이메일로 회원가입됨(닉네임은 기본 정책에 따라 랜덤, 비밀번호 로그인 불가)


###### URL Structure

`http://%HOST%/auth/oauth/facebook`


###### Parameters

| Name | Required | Description |
|---|---|---|



###### Response


###### HTTP Status

* 200


###### Sample JSON Response

	{
	  "created": "Sat, 06 Dec 2014 17:00:43 GMT",
	  "email": "iowm0818@naver.com",
	  "id": 2,
	  "modified": "Sat, 06 Dec 2014 17:31:34 GMT",
	  "name": "\uc9c4\uc7ac\uaddc",
	  "nickname": "V1MNKUVN",
	  "role": 0,
	  "status": 0,
	  "type": 1
	}


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />

<a name="user"></a>
## 2. User


<a name="user-get-id"></a>
### ``` GET ``` /users/{id} ``` user::view_user ```

###### Description

id 값에 해당하는 User 정보 받기


###### URL Structure

`http://%HOST%/users/{id}`


###### Parameters

| Name | Required | Description |
|---|---|---|


###### Response


###### HTTP Status

* 200

	
###### Sample JSON Response
    {
        "articles": 0,
        "comments": 0,
        "id": 2,
        "image": "default_user.png",
        "is_social_friend": false,
        "nickname": "YDJW56AH",
        "type": 9,
    }	

###### Errors

| Status | Code | Message |
|---|---|---|


###### Notes



<br /><br />


<a name="user-edit-me"></a>
### ``` PUT ``` /users/me ``` user::edit_user_me ```

###### Description

내 정보 수정


###### URL Structure

`http://%HOST%/users/me`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **nickname** | *Optional* | 자료 수 |
| **password** | *Optional* | 페이지 번호 |
| **image** | *Optional* | 이미지 |



<br /><br />

<a name="user-get-id-articles"></a>
### ``` GET ``` /users/{id}/articles ``` user::view_user_articles ```

###### Description

특정 사용자가 작성한 글 목록

###### URL Structure

`http://%HOST%/users/{id}/articles`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |

###### Response


###### HTTP Status

* 200

	
###### Errors

| Status | Code | Message |
|---|---|---|


###### Notes



<br /><br />


<a name="user-get-id-comments"></a>
### ``` GET ``` /users/{id}/comments ``` user::view_user_comments ```

###### Description

특정 사용자가 작성한 댓글 목록

###### URL Structure

`http://%HOST%/users/{id}/comments`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |


###### Response


###### HTTP Status

* 200


| Status | Code | Message |
|---|---|---|


###### Notes


<br /><br />


<a name="user-block"></a>
### ``` POST ``` /users/{id}/block ``` user::block_user ```

###### Description

특정 사용자 차단
전체 글 목록, 특정 글의 전체 댓글 목록에서 해당 사용자 작성글 차단됨

###### URL Structure

`http://%HOST%/users/{id}/block`


###### Parameters

| Name | Required | Description |
|---|---|---|

<br /><br />


<a name="user-unblock"></a>
### ``` DELETE ``` /users/{id}/block ``` user::unblock_user ```

###### Description

특정 사용자 차단

###### URL Structure

`http://%HOST%/users/{id}/block`


###### Parameters

| Name | Required | Description |
|---|---|---|

<br /><br />

<a name="user-subscribe"></a>
### ``` POST ``` /users/{id}/subscribe ``` user::subscribe_user ```

###### Description

특정 사용자 구독

###### URL Structure

`http://%HOST%/users/{id}/subscribe`


###### Parameters

| Name | Required | Description |
|---|---|---|

<br /><br />


<a name="user-unsubscribe"></a>
### ``` DELETE ``` /users/{id}/subscribe ``` user::unsubscribe_user ```

###### Description

특정 사용자 구독 취소

###### URL Structure

`http://%HOST%/users/{id}/subscribe`


###### Parameters

| Name | Required | Description |
|---|---|---|

<br /><br />



<a name="user-get-id-activities"></a>
### ``` GET ``` /users/{id}/activities ``` user::view_user_activities ```

###### Description

특정 사용자의 활동내역

###### URL Structure

`http://%HOST%/users/{id}/activities`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |


<br /><br />


<a name="user-subscribes"></a>
### ``` GET ``` /users/{id}/subscribes ``` user::view_user_me_subscribes ```

###### Description

특정 사용자가 구독하는 카테고리, 사용자 게시물

###### URL Structure

`http://%HOST%/users/{id}/subscribes`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |


<br /><br />


<a name="user-me-subscribes-categories-list"></a>
### ``` GET ``` /users/me/subscribes/categories ``` user::view_user_me_subscribe_category_list ```

###### Description

내가 구독하는 카테고리 목록

###### URL Structure

`http://%HOST%/users/me/subscribes/categories`


###### Parameters

| Name | Required | Description |
|---|---|---|


<br /><br />


<a name="user-subscribes-categories-list"></a>
### ``` GET ``` /users/{id}/subscribes/categories ``` user::view_user_subscribe_category_list ```

###### Description

특정 사용자가 구독하는 카테고리 목록

###### URL Structure

`http://%HOST%/users/{id}/subscribes/categories`


###### Parameters

| Name | Required | Description |
|---|---|---|


<br /><br />

<a name="user-subscribe-category"></a>
### ``` POST ``` /users/me/subscribes/categories/{category_id} ``` user::subscribe_category ```

###### Description

특정 카테고리 구독

###### URL Structure

`http://%HOST%/users/me/subscribes/categories/{category_id}`


###### Parameters

| Name | Required | Description |
|---|---|---|


###### Notes



<br /><br />



<a name="user-unsubscribe-category"></a>
### ``` DELETE ``` /users/me/subscribes/categories/{category_id} ``` user::unsubscribe_category ```

###### Description

특정 카테고리 구독 취소

###### URL Structure

`http://%HOST%/users/me/subscribes/categories/{category_id}`


###### Parameters

| Name | Required | Description |
|---|---|---|


###### Notes



<br /><br />



<a name="user-subscribes-users"></a>
### ``` GET ``` /users/{id}/subscribes/users ``` user::view_user_me_subscribe_user_list ```

###### Description

특정 사용자가 구독하는 사용자 목록

###### URL Structure

`http://%HOST%/users/{id}/subscribes/users`


###### Parameters

| Name | Required | Description |
|---|---|---|


<br /><br />



<a name="user-subscribes-categories-articles"></a>
### ``` GET ``` /users/{id}/subscribes/categories/articles ``` user::view_user_subscribes_category_articles ```

###### Description

특정 사용자가 구독하는 카테고리 게시물

###### URL Structure

`http://%HOST%/users/{id}/subscribes/categories/articles`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |


<br /><br />


<a name="user-subscribes-users-articles"></a>
### ``` GET ``` /users/{id}/subscribes/users/articles ``` user::view_user_subscribes_user_articles ```

###### Description

특정 사용자가 구독하는 사용자 게시물

###### URL Structure

`http://%HOST%/users/{id}/subscribes/users/articles`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |


<br /><br />

<a name="user-commented-articles-list"></a>
### ``` GET ``` /users/me/comments/articles ``` user::view_user_me_commented_articles```

###### Description

내가 코멘트한 글 목록

###### URL Structure

`http://%HOST%/users/{id}/subscribes/categories`


###### Parameters

| Name | Required | Description |
|---|---|---|


<br /><br />

<a name="user-commented-articles-list"></a>
### ``` GET ``` /users/{id}/comments/articles ``` user::view_user_commented_articles```

###### Description

특정 사용자가 코멘트한 글 목록

###### URL Structure

`http://%HOST%/users/{id}/subscribes/categories`


###### Parameters

| Name | Required | Description |
|---|---|---|


<br /><br />

<a name="article"></a>
## 3. Article


<a name="article-get"></a>
### ``` GET ``` /articles ``` article::list_article```

###### Description

최신 글 목록

###### URL Structure

`http://%HOST%/articles`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |
| **up** | *Optional* | emotion이 이 값보다 큼 |
| **down** | *Optional* | emotion이 이 값보다 작음 |
| **eq** | *Optional* | emotion이 이 값과 동일 |

###### Response


###### HTTP Status

* 200


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="article-get-id"></a>
### ``` GET ``` /articles/{id} ``` article::view_article ```

###### Description

특정 글 정보

###### URL Structure

`http://%HOST%/articles/{id}`


###### Parameters

| Name | Required | Description |
|---|---|---|

###### Response


###### HTTP Status

* 200



###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="article-post"></a>
### ``` POST ``` /articles/ ``` article::post_article ```

###### Description

글 등록

###### URL Structure

`http://%HOST%/articles/`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **body** | *Required* | 본문 |
| **image** | *Optional* | 이미지(아래 color 키값과 함께 사용불가, image 혹은 color 둘중 하나는 필수) |
| **color** | *Optional* | 32bit-Integer 색상 값(아래 color 키값과 함께 사용불가, image 혹은 color 둘중 하나는 필수) |
| **emotion** | *Optional* | 32bit-Intenger(특정 emotion 코드 값 emotion 이 0 보다 작으면 down 0보다 크면 업) |
| **type** | *Optional* | 글 카테고리(기본 0) |
| **format**| *Optional* | |
| **tags** | *Optioanl* | 해시태그 (ex: "#태그1 #태그2 무시될값")

###### Response


###### HTTP Status

* 201


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />

<a name="article-delete"></a>
### ``` DELETE ``` /articles/ ``` article::delete_article ```

###### Description

글 삭제

###### URL Structure

`http://%HOST%/articles/`


###### Parameters

| Name | Required | Description |
|---|---|---|



<br /><br />


<a name="article-get-id-comments"></a>
### ``` GET ``` /articles/{id}/comments ``` article::article_comments ```

###### Description

특정 글의 댓글 목록

###### URL Structure

`http://%HOST%/articles/{id}/comments`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |

###### Response


###### HTTP Status

* 200


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="article-post-id-comments"></a>
### ``` POST ``` /articles/{id}/comments ``` article::post_article ```

###### Description

특정 글에 댓글 달기

###### URL Structure

`http://%HOST%/articles/{id}/comments`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **body** | *Required* | 본문 |
| **color** | *Optional* | 32bit-Integer 색상 값 |
| **type** | *Optional* | 예비용, 현재 사용되지 않음(기본 0) |

###### Response


###### HTTP Status

* 200


###### Sample JSON Response
	{
	    "id": 2,
	    "result": true
	}


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="article-post-id-like"></a>
### ``` POST ``` /articles/{id}/like ``` article::like_article ```

###### Description

특정 글에 좋아요

###### URL Structure

`http://%HOST%/articles/{id}/like`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **type** | *Optional* | 감정 종류(기본값 0) |

###### Response



<br /><br />


<a name="article-delete-id-like"></a>
### ``` DELETE ``` /articles/{id}/like ``` article::unlike_article ```

###### Description

특정 글에 좋아요 취소

###### URL Structure

`http://%HOST%/articles/{id}/like`


###### Parameters

| Name | Required | Description |
|---|---|---|

###### Response


###### HTTP Status

* 200


###### Sample JSON Response
	{
	    "result": true
	}


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />


<a name="article-category-list"></a>
### ``` GET ``` /articles/categories/{category_id} ``` article::article_category_list```

###### Description

특정 카테고리 글 목록

###### URL Structure

`http://%HOST%/articles/categories/{category_id}`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |
| **up** | *Optional* | emotion이 이 값보다 큼 |
| **down** | *Optional* | emotion이 이 값보다 작음 |
| **eq** | *Optional* | emotion이 이 값과 동일 |

###### Response


###### HTTP Status

* 200


###### Errors

| Status | Code | Message |
|---|---|---|



###### Notes



<br /><br />

<a name="hashtag"></a>
## 4. Hashtag


<a name="hashtag-get"></a>
### ``` GET ``` /hashtags ``` hashtag::hashtag_root ```

###### Description

다중 해시태그 검색

###### URL Structure

`http://%HOST%/hashtags`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |
| **tag** | *Required* | 해시태그, 같은 키 값으로 중복해서 추가 |

###### Response
<br /><br />


<a name="hashtag-get-tag"></a>
### ``` GET ``` /hashtags/{tag} ``` hashtag::list_hashtag_articles ```

###### Description

특정 해시태그가 사용된 글 목록

###### URL Structure

`http://%HOST%/hashtags/{tag}`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |

###### Notes



<br /><br />


<a name="hashtag-search-tag"></a>
### ``` GET ``` /hashtags/search/{tag} ``` hashtag::search_hashtag ```

###### Description

입력한 해시태그와 유사한 태그를 사용한 글 목록

###### URL Structure

`http://%HOST%/hashtags/search/{tag}`


###### Parameters

| Name | Required | Description |
|---|---|---|
| **offset** | *Optional* | 자료 수 |
| **page** | *Optional* | 페이지 번호 |

###### Notes



<br /><br />



<a name="comment"></a>
## 5. Comment
<a name="comment-view"></a>
### ``` GET ``` /comments/{id} ``` comment::view_comment ```

###### Description

댓글 보기

###### URL Structure

`http://%HOST%/comments/{id}`


###### Parameters

| Name | Required | Description |
|---|---|---|

###### Response



<br /><br />


<a name="comment-delete"></a>
### ``` DELETE ``` /comments/{id} ``` comment::delete_comment ```

###### Description

댓글 삭제

###### URL Structure

`http://%HOST%/comments/{id}`


###### Parameters

| Name | Required | Description |
|---|---|---|

###### Response



<br /><br />


## 6. Report
<a name="reports-list"></a>
### ``` GET ``` /reports/ ``` report::list_report ```

###### Description

전체 신고 내역, 권한(role)이 99인 사용자만 가능

###### URL Structure

`http://%HOST%/reports`


###### Parameters

| Name | Required | Description |
|---|---|---|

###### Response



<br /><br />


<a name="reports"></a>
### ``` POST ``` /reports ``` report::post_report ```

###### Description

신고하기

###### URL Structure

`http://%HOST%/reports/`


###### Parameters

| Name | Required | Description |
|---|---|---|
| article_id | *Optioanl* | 글 id |
| comment_id | *Optioanl* | 댓글 id |
| user_id | *Optioanl* | 사용자 id(글, 댓글 id 없을 경우 필수) |
| reason | *Required* | 사유 |

###### Response



<br /><br />


<a name="reports-accept"></a>
### ``` POST ``` /reports/{id} ``` report::accept_report ```

###### Description

신고수락
권한이 99인 사용자만 가능, 사용자에 대한 신고 처리 규칙 없음.

###### URL Structure

`http://%HOST%/reports/{id}`


###### Parameters

| Name | Required | Description |
|---|---|---|

###### Response



<br /><br />


## 7. Notification
<a name="notification-list"></a>
### ``` GET ``` /notifications/ ``` notification::list_notification ```

###### Description

알림 내역

###### URL Structure

`http://%HOST%/notifications`


###### Parameters

| Name | Required | Description |
|---|---|---|

###### Response



<br /><br />