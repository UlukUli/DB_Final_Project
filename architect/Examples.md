There are some question and our expected quieries that AI shoul create

Seasonality 

Which months have the highest booking demand?
SELECT arrival_date_month, COUNT(*) AS bookings
FROM bookings
GROUP BY arrival_date_month
ORDER BY bookings DESC;

Which season is the most profitable?
SELECT arrival_date_month, SUM(adr * (stays_in_weekend_nights + stays_in_week_nights)) AS revenue
FROM bookings
GROUP BY arrival_date_month
ORDER BY revenue DESC;

How has occupancy changed over the years?
SELECT arrival_date_year, COUNT(*) AS total_bookings
FROM bookings
GROUP BY arrival_date_year
ORDER BY arrival_date_year;

Room Popularity

Which room types are booked most frequently?
SELECT r.room_code, COUNT(*) AS times_booked
FROM bookings b
JOIN rooms r ON b.reserved_room_id = r.room_id
GROUP BY r.room_code
ORDER BY times_booked DESC;

Which rooms get reassigned the most?
SELECT r.room_code, COUNT(*) AS reassign_count
FROM bookings b
JOIN rooms r ON b.assigned_room_id = r.room_id
WHERE b.assigned_room_id <> b.reserved_room_id
GROUP BY r.room_code
ORDER BY reassign_count DESC;

Which room type generates the most revenue?
SELECT r.room_code, SUM(b.adr) AS total_revenue
FROM bookings b
JOIN rooms r ON b.reserved_room_id = r.room_id
GROUP BY r.room_code
ORDER BY total_revenue DESC;

Customer Behavior 

How many repeat guests do we have?
SELECT COUNT(*) 
FROM customers
WHERE is_repeated_guest = 1;

Do repeat guests cancel more often?
SELECT c.is_repeated_guest, COUNT(*) AS cancellations
FROM bookings b
JOIN customers c ON b.customer_id = c.customer_id
WHERE b.is_canceled = 1
GROUP BY c.is_repeated_guest;

Cancellations 

Which months have the most cancellations?
SELECT arrival_date_month, COUNT(*) AS cancellations
FROM bookings
WHERE is_canceled = 1
GROUP BY arrival_date_month
ORDER BY cancellations DESC;

Which meal plans correlate with higher cancellation rates?
SELECT m.meal, COUNT(*) AS cancellations
FROM bookings b
JOIN meals m ON b.meal_id = m.meal_id
WHERE b.is_canceled = 1
GROUP BY m.meal
ORDER BY cancellations DESC;

Meal Plans 

Which meal plan is the most popular?
SELECT m.meal, COUNT(*) AS usage_count
FROM bookings b
JOIN meals m ON m.meal_id = b.meal_id
GROUP BY m.meal
ORDER BY usage_count DESC;

Is there seasonality in meal plan preferences?
SELECT arrival_date_month, m.meal, COUNT(*) AS count_meal
FROM bookings b
JOIN meals m ON b.meal_id = m.meal_id
GROUP BY arrival_date_month, m.meal
ORDER BY arrival_date_month, count_meal DESC;

Market Segments & Channels

Which market segments generate the most bookings?
SELECT ms.market_segment, COUNT(*) AS total
FROM bookings b
JOIN market_segments ms ON b.market_segment_id = ms.market_segment_id
GROUP BY ms.market_segment
ORDER BY total DESC;

Which channels bring the highest-paying guests?

SELECT dc.distribution_channel, AVG(b.adr) AS avg_adr
FROM bookings b
JOIN distribution_channels dc ON b.distribution_channel_id = dc.distribution_channel_id
GROUP BY dc.distribution_channel
ORDER BY avg_adr DESC;
