SELECT prod_id, FORMAT_NUMBER(avg_rating, 2) AS 
avg_rating 
    FROM (SELECT prod_id, AVG(rating) AS avg_rating,  
            COUNT(*) AS num  
            FROM ratings 
            GROUP BY prod_id) rated 
    WHERE num >= 50 
    ORDER BY avg_rating ASC
    LIMIT 1;

SELECT EXPLODE(NGRAMS(SENTENCES(LOWER(message)), 3, 5))  
    AS trigrams
    FROM ratings 
    WHERE prod_id = 1274673; 

SELECT distinct message 
     FROM ratings  
     WHERE prod_id = 1274673  
       AND message LIKE '%red%'  
     LIMIT 3; 

SELECT * 
	FROM products
	WHERE prod_id = 1274673;

CREATE TABLE cart_items AS
SELECT cookie, prod_id FROM
	(SELECT cookie, REGEXP_EXTRACT(request, '/cart/additem?productid=[0-9]+', 0) AS prod_id
	FROM web_logs
	WHERE request REGEXP '/cart/additem?productid=[0-9]+') prod_id
	GROUP BY prod_id, cookie;