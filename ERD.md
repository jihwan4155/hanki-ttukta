# Gwantong ERD

## 1. 개요
이 ERD는 제공된 사진을 기준으로 `gwantong` 프로젝트의 도메인 모델을 텍스트 방식으로 정리한 것입니다.

## 2. 도메인 구분
- 레시피 추천 도메인
- 마켓 도메인

---

## 3. 테이블 정의

### 사용자 (users)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `user_id` | int | PK | |
| `username` | varchar | | |
| `email` | varchar | | |
| `password_hash` | hash | | |
| `profile_image` | text | | |
| `household_size` | int | | |
| `dietary_preference` | choice | | |
| `allergy_info` | varchar | | |
| `goal` | varchar | | |
| `alert_days_before_expiry` | int | | |
| `created_at` | datetime | | |
| `updated_at` | datetime | | |

### 냉장고 속 식재료 (fridge_ingredients)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `fridge_ingredient_id` | int | PK | |
| `user_id` | int | FK | `users.user_id` |
| `ingredient_id` | int | FK | `ingredients.ingredient_id` |
| `quantity` | float | | |
| `unit` | varchar | | |
| `purchase_date` | date | | |
| `expiry_date` | date | | |
| `memo` | text | | |
| `created_at` | datetime | | |
| `updated_at` | datetime | | |

### 식재료 (ingredients)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `ingredient_id` | int | PK | |
| `name` | varchar | unique | |
| `category` | varchar | | |
| `default_unit` | varchar | | |
| `expiry_days` | int | | |
| `calories_per_100g` | float | | |
| `protein_per_100g` | float | | |
| `fat_per_100g` | float | | |
| `carb_per_100g` | float | | |
| `unit_per_100g` | float | | |

### 식재료 가격 이력 (ingredient_price_histories)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `history_id` | int | PK | |
| `market_id` | int | FK | `markets.market_id` |
| `ingredient_id` | int | FK | `ingredients.ingredient_id` |
| `price` | float | | |
| `quantity` | float | | |
| `unit` | varchar | | |
| `price_per_100g` | float | nullable | |
| `recorded_date` | date | | |

### 마켓 (markets)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `market_id` | int | PK | |
| `name` | varchar | | |
| `province` | varchar | | |
| `city` | varchar | | |
| `latitude` | float | | |
| `longitude` | float | | |

### 식재료 별칭 (ingredient_aliases)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `alias_id` | int | PK | |
| `ingredient_id` | int | FK | `ingredients.ingredient_id` |
| `alias_name` | varchar | | |
| `normalized_alias` | varchar | | |
| `created_at` | datetime | | |
| `updated_at` | datetime | | |

### 레시피 (recipes)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `recipe_id` | int | PK | |
| `title` | varchar | | |
| `source_id` | varchar | | |
| `source_name` | varchar | | |
| `source_url` | varchar | | |
| `description` | varchar | | |
| `cooking_type` | varchar | | 요리 종류. 예: 밑반찬, 메인반찬, 국/탕, 차/음료/술 |
| `how_to_cook` | text | | 실제 조리 순서. 예: `1. 재료를 손질한다.\n2. 팬에 굽는다.` 외부 레시피 상세 페이지에서 크롤링 후 저장될 수 있음. `굽기`, `볶음`, `끓이기` 같은 요리 방법 분류값은 저장하지 않음 |
| `cook_time` | varchar | | |
| `cook_time_minutes` | int | | nullable |
| `difficulty` | varchar | | |
| `servings` | int | | |
| `cover_image` | image | | |
| `cover_image_url` | URL/Text | | 레시피 대표 이미지 URL. 외부 레시피 상세 페이지에서 크롤링 후 저장될 수 있음 |
| `is_lm_generated` | boolean | | |
| `created_at` | datetime | | |

### 레시피-식재료 (recipe_ingredients)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `recipe_ingredient_id` | int | PK | |
| `recipe_id` | int | FK | `recipes.recipe_id` |
| `ingredient_id` | int | FK | `ingredients.ingredient_id` |
| `quantity` | float | | |
| `unit` | varchar | | |
| `amount_text` | varchar | | |
| `group_name` | varchar | | |
| `is_optional` | boolean | | |
| `substitute_hint` | varchar | | |
| `original_name` | varchar | | |
| `original_text` | text | | |

### 추천 레시피 좋아요 (like_recipes)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `like_recipe_id` | int | PK | |
| `user_id` | int | FK | `users.user_id` |
| `recipe_id` | int | FK | `recipes.recipe_id` |
| unique constraint | | | (`user_id`, `recipe_id`) |

### 게시글 (posts)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `post_id` | int | PK | |
| `author_id` | int | FK | `users.user_id` |
| `title` | varchar | | |
| `image` | text | | |
| `content` | text | | |
| `created_at` | datetime | | |
| `updated_at` | datetime | | |

### 게시글 식재료 (post_ingredients)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `post_ingredient_id` | int | PK | |
| `post_id` | int | FK | `posts.post_id` |
| `ingredient_id` | int | FK | `ingredients.ingredient_id` |
| `quantity` | float | | |
| `unit` | varchar | | |

### 게시글 좋아요 (like_posts)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `like_post_id` | int | PK | |
| `user_id` | int | FK | `users.user_id` |
| `post_id` | int | FK | `posts.post_id` |
| unique constraint | | | (`user_id`, `post_id`) |

### 리뷰 (reviews)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `review_id` | int | PK | |
| `post_id` | int | FK | `posts.post_id` |
| `user_id` | int | FK | `users.user_id` |
| `rating` | int | | |
| unique constraint | | | (`post_id`, `user_id`) |

### 댓글 (comments)
| 컬럼 | 타입 | 제약 | 비고 |
|---|---|---|---|
| `comment_id` | int | PK | |
| `user_id` | int | FK | `users.user_id` |
| `post_id` | int | FK | `posts.post_id` |
| `content` | text | | |
| `created_at` | datetime | | |
| `updated_at` | datetime | | |

---

## 4. 관계 요약
- users 1 --- * fridge_ingredients
- users 1 --- * like_recipes
- users 1 --- * like_posts
- users 1 --- * posts
- users 1 --- * reviews
- users 1 --- * comments
- ingredients 1 --- * fridge_ingredients
- ingredients 1 --- * recipe_ingredients
- ingredients 1 --- * post_ingredients
- ingredients 1 --- * ingredient_price_histories
- ingredients 1 --- * ingredient_aliases
- recipes 1 --- * recipe_ingredients
- posts 1 --- * post_ingredients
- markets 1 --- * ingredient_price_histories

---

## 5. 텍스트 ERD 다이어그램
```
[users] 1──* [fridge_ingredients]
[users] 1──* [like_recipes]
[users] 1──* [like_posts]
[users] 1──* [posts]
[users] 1──* [reviews]
[users] 1──* [comments]

[ingredients] 1──* [fridge_ingredients]
[ingredients] 1──* [recipe_ingredients]
[ingredients] 1──* [post_ingredients]
[ingredients] 1──* [ingredient_price_histories]
[ingredients] 1──* [ingredient_aliases]

[recipes] 1──* [recipe_ingredients]
[posts] 1──* [post_ingredients]
[markets] 1──* [ingredient_price_histories]

[posts] 1──* [reviews]
```
