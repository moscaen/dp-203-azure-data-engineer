# Exam #7

    # of Correct Answers/Total Questions: 37/45

    Objectives Percentage (Corrects/Number Questions)
    Design and implement data storage 61.54 % (8/13)
    Design and develop data processing 93.75 % (15/16)
    Design and implement data security 81.82 % (9/11)
    Monitor and optimize data storage and data processing 100 % (5/5)

## Design and implement data storage

### Design a partition strategy

- Sharding partitions data horizontally to distribute data across multiple databases in a scaled-out design. The schema must be the same through all the dbs involved. Sharding helps to minimize the size of individual dbs which hel to impreve transacational process performance.

### Design the serving layer

- Azure stream analytics allows you to analyze streaming datasets and integrate them with stream analytics engine to provide real-time analysis.

### Implement logical data structures

- GENERATED ALWAYS AS ROW START
- GENERATED ALWAYS AS ROW END
- PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
- A system-versioned temporal table must have a primary key defined and have exactly one PERIOD FOR SYSTEM_TIME defined with two datetime2 columns, declared as GENERATED ALWAYS AS ROW START / END

## Design and develop data processing

### Design and develop a stream proessing solution

|           | Overlap | Filters period with no data | Check time duration between events |
| --------- | ------- | --------------------------- | ---------------------------------- |
| Tumbling  | N       | N                           | N                                  |
| Hopping   | Y       | N                           | N                                  |
| Windowing | Y       | Y                           | N                                  |
| Session   | Y       | Y                           | Y                                  |

## Design and implement data Security

### Design security for data policies and standards

|                               | Affected by regenerate access key |
| ----------------------------- | --------------------------------- |
| Shared Key                    | Y                                 |
| Service SAS                   | Y                                 |
| Account SAS                   | Y                                 |
| User delegation SAS           | N                                 |
| Azure AD                      | N                                 |
| Anonymous public read acccess | N                                 |
