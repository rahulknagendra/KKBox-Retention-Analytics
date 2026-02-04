# Data Dictionary

## Tables

### train
Users in the prediction set with churn labels.

- msno (STRING): User ID, primary key
- is_churn (INTEGER): Target variable. 1 = churned, 0 = retained

### members
User demographics and registration info.

- msno (STRING): User ID, joins to train
- city (INTEGER): City code
- bd (INTEGER): Age. Values 0/negative (67%), below 13, and over 100 (0.1%) are treated as invalid.
- gender (STRING): "male", "female", or NULL. 65% missing.
- registered_via (INTEGER): Registration channel code. Top channels: 4, 3, 9, 7.
- registration_init_time (INTEGER): Registration date in YYYYMMDD format,
    - Values after 20170131 produce invalid tenure (18,152 users, 1.9%)

### transactions
Subscription transaction history (Jan 2015 - Mar 2017).

- msno (STRING): User ID, joins to train
- payment_method_id (INTEGER): Payment method code
- payment_plan_days (INTEGER): Subscription length in days. 85% are 30-day plans.
- plan_list_price (INTEGER): Listed price (TWD)
- actual_amount_paid (INTEGER): Amount paid (TWD)
- is_auto_renew (INTEGER): 1 = auto-renew enabled, 0 = not enabled
- transaction_date (INTEGER): Transaction date in YYYYMMDD format
- membership_expire_date (INTEGER): Expiration date in YYYYMMDD format
- is_cancel (INTEGER): 1 = cancellation transaction, 0 = not cancellation

### user_logs
Daily listening activity (Jan 2015 - Feb 2017). One record per user per day.

- msno (STRING): User ID, joins to train
- date (INTEGER): Date in YYYYMMDD format
- num_25 (INTEGER): Songs played to 25% completion
- num_50 (INTEGER): Songs played to 50% completion
- num_75 (INTEGER): Songs played to 75% completion
- num_985 (INTEGER): Songs played to 98.5% completion
- num_100 (INTEGER): Songs played to 100% completion
- num_unq (INTEGER): Unique songs played
- total_secs (FLOAT): Total listening time in seconds. Values negative or over 57,600 (16 hours) are invalid (0.06% of records).

## Row Counts

- train: 970,960
- members: 6,769,473
- transactions: 1,431,009
- user_logs: 392,106,543

## Coverage

Percentage of train users with data in each table:

- transactions: 96.1%
- members: 88.7%
- user_logs: 87.6%