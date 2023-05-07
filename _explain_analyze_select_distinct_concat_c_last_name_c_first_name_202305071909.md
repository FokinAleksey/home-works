|EXPLAIN|
|-------|
|-> Table scan on <temporary>  (cost=2.5..2.5 rows=0) (actual time=12254..12254 rows=391 loops=1)
    -> Temporary table with deduplication  (cost=0..0 rows=0) (actual time=12254..12254 rows=391 loops=1)
        -> Window aggregate with buffering: sum(payment.amount) OVER (PARTITION BY c.customer_id,f.title )   (actual time=4949..11849 rows=642000 loops=1)
            -> Sort: c.customer_id, f.title  (actual time=4949..5147 rows=642000 loops=1)
                -> Stream results  (cost=22.6e+6 rows=16.5e+6) (actual time=0.783..3542 rows=642000 loops=1)
                    -> Nested loop inner join  (cost=22.6e+6 rows=16.5e+6) (actual time=0.778..2864 rows=642000 loops=1)
                        -> Nested loop inner join  (cost=20.9e+6 rows=16.5e+6) (actual time=0.775..2488 rows=642000 loops=1)
                            -> Nested loop inner join  (cost=19.3e+6 rows=16.5e+6) (actual time=0.769..2065 rows=642000 loops=1)
                                -> Inner hash join (no condition)  (cost=1.65e+6 rows=16.5e+6) (actual time=0.758..145 rows=634000 loops=1)
                                    -> Filter: (cast(p.payment_date as date) = '2005-07-30')  (cost=1.72 rows=16500) (actual time=0.0553..13.7 rows=634 loops=1)
                                        -> Table scan on p  (cost=1.72 rows=16500) (actual time=0.0347..8.23 rows=16044 loops=1)
                                    -> Hash
                                        -> Covering index scan on f using idx_title  (cost=112 rows=1000) (actual time=0.0419..0.53 rows=1000 loops=1)
                                -> Covering index lookup on r using rental_date (rental_date=p.payment_date)  (cost=0.969 rows=1) (actual time=0.00189..0.0027 rows=1.01 loops=634000)
                            -> Single-row index lookup on c using PRIMARY (customer_id=r.customer_id)  (cost=250e-6 rows=1) (actual time=308e-6..352e-6 rows=1 loops=642000)
                        -> Single-row covering index lookup on i using PRIMARY (inventory_id=r.inventory_id)  (cost=250e-6 rows=1) (actual time=246e-6..291e-6 rows=1 loops=642000)
|
