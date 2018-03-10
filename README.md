
# Unit 6 - What's the Weather Like?

In this example, you'll be creating a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. To accomplish this, you'll be utilizing a simple Python library, the OpenWeatherMap API, and a little common sense to create a representative model of weather across world cities.
Your objective is to build a series of scatter plots to showcase the following relationships:
* Temperature (F) vs. Latitude
* Humidity (%) vs. Latitude
* Cloudiness (%) vs. Latitude
* Wind Speed (mph) vs. Latitude

## Observations

* Average temperature does appear to increase the closer the city is to the equator
* Humidity does not show a correlation to any particular line of latitude
* Cloudiness does not appear to show a correltaion to any particular line of latitude
* Across most lines of latitude, windspeed seems to average below 15mph

* Accuracy: Data was run on March 8th at 8pm Central Time


```python
# Dependencies
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import requests
import time
from citipy import citipy
from pprint import pprint
from config import api_key
```


```python
# import coordinates
coordinates = pd.read_excel('lat_long_generator.xlsx', sheetname='Sheet1', header=None)
coordinates = coordinates.rename(columns={0:'Latitude', 1:'Longitude'})

```


```python
# find closest city
coordinates["City"] = ""

for index,row in coordinates.iterrows():
    city = citipy.nearest_city(row["Latitude"],row["Longitude"])
    coordinates.set_value(index,"City",city.city_name)
coordinates.shape
```




    (2500, 3)




```python
# Look for duplicates and remove when found
sort_dupes = coordinates.sort_values('City', ascending=False)
city_subset = sort_dupes.drop_duplicates('City', keep='first').sort_index()

```


```python
# Save config information and define variables
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

# Counter
row_count = 0

for index,row in city_subset.iterrows():
    time.sleep(1) 
    city = row["City"]
    query_url = f"{url}appid={api_key}&q={city}&units={units}"
    print("Now retrieving city number " + str(row_count))
    city_wx_response = requests.get(query_url)
    city_wx = city_wx_response.json()
    city_subset.set_value(index,"Temperature",city_wx.get("main",{}).get("temp_max"))
    city_subset.set_value(index,"Humidity",city_wx.get("main",{}).get("humidity"))
    city_subset.set_value(index,"Cloudiness",city_wx.get("clouds",{}).get("all"))
    city_subset.set_value(index,"Wind speed",city_wx.get("wind",{}).get("speed"))
    
    row_count += 1


```

    Now retrieving city number 0
    Now retrieving city number 1
    Now retrieving city number 2
    Now retrieving city number 3
    Now retrieving city number 4
    Now retrieving city number 5
    Now retrieving city number 6
    Now retrieving city number 7
    Now retrieving city number 8
    Now retrieving city number 9
    Now retrieving city number 10
    Now retrieving city number 11
    Now retrieving city number 12
    Now retrieving city number 13
    Now retrieving city number 14
    Now retrieving city number 15
    Now retrieving city number 16
    Now retrieving city number 17
    Now retrieving city number 18
    Now retrieving city number 19
    Now retrieving city number 20
    Now retrieving city number 21
    Now retrieving city number 22
    Now retrieving city number 23
    Now retrieving city number 24
    Now retrieving city number 25
    Now retrieving city number 26
    Now retrieving city number 27
    Now retrieving city number 28
    Now retrieving city number 29
    Now retrieving city number 30
    Now retrieving city number 31
    Now retrieving city number 32
    Now retrieving city number 33
    Now retrieving city number 34
    Now retrieving city number 35
    Now retrieving city number 36
    Now retrieving city number 37
    Now retrieving city number 38
    Now retrieving city number 39
    Now retrieving city number 40
    Now retrieving city number 41
    Now retrieving city number 42
    Now retrieving city number 43
    Now retrieving city number 44
    Now retrieving city number 45
    Now retrieving city number 46
    Now retrieving city number 47
    Now retrieving city number 48
    Now retrieving city number 49
    Now retrieving city number 50
    Now retrieving city number 51
    Now retrieving city number 52
    Now retrieving city number 53
    Now retrieving city number 54
    Now retrieving city number 55
    Now retrieving city number 56
    Now retrieving city number 57
    Now retrieving city number 58
    Now retrieving city number 59
    Now retrieving city number 60
    Now retrieving city number 61
    Now retrieving city number 62
    Now retrieving city number 63
    Now retrieving city number 64
    Now retrieving city number 65
    Now retrieving city number 66
    Now retrieving city number 67
    Now retrieving city number 68
    Now retrieving city number 69
    Now retrieving city number 70
    Now retrieving city number 71
    Now retrieving city number 72
    Now retrieving city number 73
    Now retrieving city number 74
    Now retrieving city number 75
    Now retrieving city number 76
    Now retrieving city number 77
    Now retrieving city number 78
    Now retrieving city number 79
    Now retrieving city number 80
    Now retrieving city number 81
    Now retrieving city number 82
    Now retrieving city number 83
    Now retrieving city number 84
    Now retrieving city number 85
    Now retrieving city number 86
    Now retrieving city number 87
    Now retrieving city number 88
    Now retrieving city number 89
    Now retrieving city number 90
    Now retrieving city number 91
    Now retrieving city number 92
    Now retrieving city number 93
    Now retrieving city number 94
    Now retrieving city number 95
    Now retrieving city number 96
    Now retrieving city number 97
    Now retrieving city number 98
    Now retrieving city number 99
    Now retrieving city number 100
    Now retrieving city number 101
    Now retrieving city number 102
    Now retrieving city number 103
    Now retrieving city number 104
    Now retrieving city number 105
    Now retrieving city number 106
    Now retrieving city number 107
    Now retrieving city number 108
    Now retrieving city number 109
    Now retrieving city number 110
    Now retrieving city number 111
    Now retrieving city number 112
    Now retrieving city number 113
    Now retrieving city number 114
    Now retrieving city number 115
    Now retrieving city number 116
    Now retrieving city number 117
    Now retrieving city number 118
    Now retrieving city number 119
    Now retrieving city number 120
    Now retrieving city number 121
    Now retrieving city number 122
    Now retrieving city number 123
    Now retrieving city number 124
    Now retrieving city number 125
    Now retrieving city number 126
    Now retrieving city number 127
    Now retrieving city number 128
    Now retrieving city number 129
    Now retrieving city number 130
    Now retrieving city number 131
    Now retrieving city number 132
    Now retrieving city number 133
    Now retrieving city number 134
    Now retrieving city number 135
    Now retrieving city number 136
    Now retrieving city number 137
    Now retrieving city number 138
    Now retrieving city number 139
    Now retrieving city number 140
    Now retrieving city number 141
    Now retrieving city number 142
    Now retrieving city number 143
    Now retrieving city number 144
    Now retrieving city number 145
    Now retrieving city number 146
    Now retrieving city number 147
    Now retrieving city number 148
    Now retrieving city number 149
    Now retrieving city number 150
    Now retrieving city number 151
    Now retrieving city number 152
    Now retrieving city number 153
    Now retrieving city number 154
    Now retrieving city number 155
    Now retrieving city number 156
    Now retrieving city number 157
    Now retrieving city number 158
    Now retrieving city number 159
    Now retrieving city number 160
    Now retrieving city number 161
    Now retrieving city number 162
    Now retrieving city number 163
    Now retrieving city number 164
    Now retrieving city number 165
    Now retrieving city number 166
    Now retrieving city number 167
    Now retrieving city number 168
    Now retrieving city number 169
    Now retrieving city number 170
    Now retrieving city number 171
    Now retrieving city number 172
    Now retrieving city number 173
    Now retrieving city number 174
    Now retrieving city number 175
    Now retrieving city number 176
    Now retrieving city number 177
    Now retrieving city number 178
    Now retrieving city number 179
    Now retrieving city number 180
    Now retrieving city number 181
    Now retrieving city number 182
    Now retrieving city number 183
    Now retrieving city number 184
    Now retrieving city number 185
    Now retrieving city number 186
    Now retrieving city number 187
    Now retrieving city number 188
    Now retrieving city number 189
    Now retrieving city number 190
    Now retrieving city number 191
    Now retrieving city number 192
    Now retrieving city number 193
    Now retrieving city number 194
    Now retrieving city number 195
    Now retrieving city number 196
    Now retrieving city number 197
    Now retrieving city number 198
    Now retrieving city number 199
    Now retrieving city number 200
    Now retrieving city number 201
    Now retrieving city number 202
    Now retrieving city number 203
    Now retrieving city number 204
    Now retrieving city number 205
    Now retrieving city number 206
    Now retrieving city number 207
    Now retrieving city number 208
    Now retrieving city number 209
    Now retrieving city number 210
    Now retrieving city number 211
    Now retrieving city number 212
    Now retrieving city number 213
    Now retrieving city number 214
    Now retrieving city number 215
    Now retrieving city number 216
    Now retrieving city number 217
    Now retrieving city number 218
    Now retrieving city number 219
    Now retrieving city number 220
    Now retrieving city number 221
    Now retrieving city number 222
    Now retrieving city number 223
    Now retrieving city number 224
    Now retrieving city number 225
    Now retrieving city number 226
    Now retrieving city number 227
    Now retrieving city number 228
    Now retrieving city number 229
    Now retrieving city number 230
    Now retrieving city number 231
    Now retrieving city number 232
    Now retrieving city number 233
    Now retrieving city number 234
    Now retrieving city number 235
    Now retrieving city number 236
    Now retrieving city number 237
    Now retrieving city number 238
    Now retrieving city number 239
    Now retrieving city number 240
    Now retrieving city number 241
    Now retrieving city number 242
    Now retrieving city number 243
    Now retrieving city number 244
    Now retrieving city number 245
    Now retrieving city number 246
    Now retrieving city number 247
    Now retrieving city number 248
    Now retrieving city number 249
    Now retrieving city number 250
    Now retrieving city number 251
    Now retrieving city number 252
    Now retrieving city number 253
    Now retrieving city number 254
    Now retrieving city number 255
    Now retrieving city number 256
    Now retrieving city number 257
    Now retrieving city number 258
    Now retrieving city number 259
    Now retrieving city number 260
    Now retrieving city number 261
    Now retrieving city number 262
    Now retrieving city number 263
    Now retrieving city number 264
    Now retrieving city number 265
    Now retrieving city number 266
    Now retrieving city number 267
    Now retrieving city number 268
    Now retrieving city number 269
    Now retrieving city number 270
    Now retrieving city number 271
    Now retrieving city number 272
    Now retrieving city number 273
    Now retrieving city number 274
    Now retrieving city number 275
    Now retrieving city number 276
    Now retrieving city number 277
    Now retrieving city number 278
    Now retrieving city number 279
    Now retrieving city number 280
    Now retrieving city number 281
    Now retrieving city number 282
    Now retrieving city number 283
    Now retrieving city number 284
    Now retrieving city number 285
    Now retrieving city number 286
    Now retrieving city number 287
    Now retrieving city number 288
    Now retrieving city number 289
    Now retrieving city number 290
    Now retrieving city number 291
    Now retrieving city number 292
    Now retrieving city number 293
    Now retrieving city number 294
    Now retrieving city number 295
    Now retrieving city number 296
    Now retrieving city number 297
    Now retrieving city number 298
    Now retrieving city number 299
    Now retrieving city number 300
    Now retrieving city number 301
    Now retrieving city number 302
    Now retrieving city number 303
    Now retrieving city number 304
    Now retrieving city number 305
    Now retrieving city number 306
    Now retrieving city number 307
    Now retrieving city number 308
    Now retrieving city number 309
    Now retrieving city number 310
    Now retrieving city number 311
    Now retrieving city number 312
    Now retrieving city number 313
    Now retrieving city number 314
    Now retrieving city number 315
    Now retrieving city number 316
    Now retrieving city number 317
    Now retrieving city number 318
    Now retrieving city number 319
    Now retrieving city number 320
    Now retrieving city number 321
    Now retrieving city number 322
    Now retrieving city number 323
    Now retrieving city number 324
    Now retrieving city number 325
    Now retrieving city number 326
    Now retrieving city number 327
    Now retrieving city number 328
    Now retrieving city number 329
    Now retrieving city number 330
    Now retrieving city number 331
    Now retrieving city number 332
    Now retrieving city number 333
    Now retrieving city number 334
    Now retrieving city number 335
    Now retrieving city number 336
    Now retrieving city number 337
    Now retrieving city number 338
    Now retrieving city number 339
    Now retrieving city number 340
    Now retrieving city number 341
    Now retrieving city number 342
    Now retrieving city number 343
    Now retrieving city number 344
    Now retrieving city number 345
    Now retrieving city number 346
    Now retrieving city number 347
    Now retrieving city number 348
    Now retrieving city number 349
    Now retrieving city number 350
    Now retrieving city number 351
    Now retrieving city number 352
    Now retrieving city number 353
    Now retrieving city number 354
    Now retrieving city number 355
    Now retrieving city number 356
    Now retrieving city number 357
    Now retrieving city number 358
    Now retrieving city number 359
    Now retrieving city number 360
    Now retrieving city number 361
    Now retrieving city number 362
    Now retrieving city number 363
    Now retrieving city number 364
    Now retrieving city number 365
    Now retrieving city number 366
    Now retrieving city number 367
    Now retrieving city number 368
    Now retrieving city number 369
    Now retrieving city number 370
    Now retrieving city number 371
    Now retrieving city number 372
    Now retrieving city number 373
    Now retrieving city number 374
    Now retrieving city number 375
    Now retrieving city number 376
    Now retrieving city number 377
    Now retrieving city number 378
    Now retrieving city number 379
    Now retrieving city number 380
    Now retrieving city number 381
    Now retrieving city number 382
    Now retrieving city number 383
    Now retrieving city number 384
    Now retrieving city number 385
    Now retrieving city number 386
    Now retrieving city number 387
    Now retrieving city number 388
    Now retrieving city number 389
    Now retrieving city number 390
    Now retrieving city number 391
    Now retrieving city number 392
    Now retrieving city number 393
    Now retrieving city number 394
    Now retrieving city number 395
    Now retrieving city number 396
    Now retrieving city number 397
    Now retrieving city number 398
    Now retrieving city number 399
    Now retrieving city number 400
    Now retrieving city number 401
    Now retrieving city number 402
    Now retrieving city number 403
    Now retrieving city number 404
    Now retrieving city number 405
    Now retrieving city number 406
    Now retrieving city number 407
    Now retrieving city number 408
    Now retrieving city number 409
    Now retrieving city number 410
    Now retrieving city number 411
    Now retrieving city number 412
    Now retrieving city number 413
    Now retrieving city number 414
    Now retrieving city number 415
    Now retrieving city number 416
    Now retrieving city number 417
    Now retrieving city number 418
    Now retrieving city number 419
    Now retrieving city number 420
    Now retrieving city number 421
    Now retrieving city number 422
    Now retrieving city number 423
    Now retrieving city number 424
    Now retrieving city number 425
    Now retrieving city number 426
    Now retrieving city number 427
    Now retrieving city number 428
    Now retrieving city number 429
    Now retrieving city number 430
    Now retrieving city number 431
    Now retrieving city number 432
    Now retrieving city number 433
    Now retrieving city number 434
    Now retrieving city number 435
    Now retrieving city number 436
    Now retrieving city number 437
    Now retrieving city number 438
    Now retrieving city number 439
    Now retrieving city number 440
    Now retrieving city number 441
    Now retrieving city number 442
    Now retrieving city number 443
    Now retrieving city number 444
    Now retrieving city number 445
    Now retrieving city number 446
    Now retrieving city number 447
    Now retrieving city number 448
    Now retrieving city number 449
    Now retrieving city number 450
    Now retrieving city number 451
    Now retrieving city number 452
    Now retrieving city number 453
    Now retrieving city number 454
    Now retrieving city number 455
    Now retrieving city number 456
    Now retrieving city number 457
    Now retrieving city number 458
    Now retrieving city number 459
    Now retrieving city number 460
    Now retrieving city number 461
    Now retrieving city number 462
    Now retrieving city number 463
    Now retrieving city number 464
    Now retrieving city number 465
    Now retrieving city number 466
    Now retrieving city number 467
    Now retrieving city number 468
    Now retrieving city number 469
    Now retrieving city number 470
    Now retrieving city number 471
    Now retrieving city number 472
    Now retrieving city number 473
    Now retrieving city number 474
    Now retrieving city number 475
    Now retrieving city number 476
    Now retrieving city number 477
    Now retrieving city number 478
    Now retrieving city number 479
    Now retrieving city number 480
    Now retrieving city number 481
    Now retrieving city number 482
    Now retrieving city number 483
    Now retrieving city number 484
    Now retrieving city number 485
    Now retrieving city number 486
    Now retrieving city number 487
    Now retrieving city number 488
    Now retrieving city number 489
    Now retrieving city number 490
    Now retrieving city number 491
    Now retrieving city number 492
    Now retrieving city number 493
    Now retrieving city number 494
    Now retrieving city number 495
    Now retrieving city number 496
    Now retrieving city number 497
    Now retrieving city number 498
    Now retrieving city number 499
    Now retrieving city number 500
    Now retrieving city number 501
    Now retrieving city number 502
    Now retrieving city number 503
    Now retrieving city number 504
    Now retrieving city number 505
    Now retrieving city number 506
    Now retrieving city number 507
    Now retrieving city number 508
    Now retrieving city number 509
    Now retrieving city number 510
    Now retrieving city number 511
    Now retrieving city number 512
    Now retrieving city number 513
    Now retrieving city number 514
    Now retrieving city number 515
    Now retrieving city number 516
    Now retrieving city number 517
    Now retrieving city number 518
    Now retrieving city number 519
    Now retrieving city number 520
    Now retrieving city number 521
    Now retrieving city number 522
    Now retrieving city number 523
    Now retrieving city number 524
    Now retrieving city number 525
    Now retrieving city number 526
    Now retrieving city number 527
    Now retrieving city number 528
    Now retrieving city number 529
    Now retrieving city number 530
    Now retrieving city number 531
    Now retrieving city number 532
    Now retrieving city number 533
    Now retrieving city number 534
    Now retrieving city number 535
    Now retrieving city number 536
    Now retrieving city number 537
    Now retrieving city number 538
    Now retrieving city number 539
    Now retrieving city number 540
    Now retrieving city number 541
    Now retrieving city number 542
    Now retrieving city number 543
    Now retrieving city number 544
    Now retrieving city number 545
    Now retrieving city number 546
    Now retrieving city number 547
    Now retrieving city number 548
    Now retrieving city number 549
    Now retrieving city number 550
    Now retrieving city number 551
    Now retrieving city number 552
    Now retrieving city number 553
    Now retrieving city number 554
    Now retrieving city number 555
    Now retrieving city number 556
    Now retrieving city number 557
    Now retrieving city number 558
    Now retrieving city number 559
    Now retrieving city number 560
    Now retrieving city number 561
    Now retrieving city number 562
    Now retrieving city number 563
    Now retrieving city number 564
    Now retrieving city number 565
    Now retrieving city number 566
    Now retrieving city number 567
    Now retrieving city number 568
    Now retrieving city number 569
    Now retrieving city number 570
    Now retrieving city number 571
    Now retrieving city number 572
    Now retrieving city number 573
    Now retrieving city number 574
    Now retrieving city number 575
    Now retrieving city number 576
    Now retrieving city number 577
    Now retrieving city number 578
    Now retrieving city number 579
    Now retrieving city number 580
    Now retrieving city number 581
    Now retrieving city number 582
    Now retrieving city number 583
    Now retrieving city number 584
    Now retrieving city number 585
    Now retrieving city number 586
    Now retrieving city number 587
    Now retrieving city number 588
    Now retrieving city number 589
    Now retrieving city number 590
    Now retrieving city number 591
    Now retrieving city number 592
    Now retrieving city number 593
    Now retrieving city number 594
    Now retrieving city number 595
    Now retrieving city number 596
    Now retrieving city number 597
    Now retrieving city number 598
    Now retrieving city number 599
    Now retrieving city number 600
    Now retrieving city number 601
    Now retrieving city number 602
    Now retrieving city number 603
    Now retrieving city number 604
    Now retrieving city number 605
    Now retrieving city number 606
    Now retrieving city number 607
    Now retrieving city number 608
    Now retrieving city number 609
    Now retrieving city number 610
    Now retrieving city number 611
    Now retrieving city number 612
    Now retrieving city number 613
    Now retrieving city number 614
    Now retrieving city number 615
    Now retrieving city number 616
    Now retrieving city number 617
    Now retrieving city number 618
    Now retrieving city number 619
    Now retrieving city number 620
    Now retrieving city number 621
    Now retrieving city number 622
    Now retrieving city number 623
    Now retrieving city number 624
    Now retrieving city number 625
    Now retrieving city number 626
    Now retrieving city number 627
    Now retrieving city number 628
    Now retrieving city number 629
    Now retrieving city number 630
    Now retrieving city number 631
    Now retrieving city number 632
    Now retrieving city number 633
    Now retrieving city number 634
    Now retrieving city number 635
    Now retrieving city number 636
    Now retrieving city number 637
    Now retrieving city number 638
    Now retrieving city number 639
    Now retrieving city number 640
    Now retrieving city number 641
    Now retrieving city number 642
    Now retrieving city number 643
    Now retrieving city number 644
    Now retrieving city number 645
    Now retrieving city number 646
    Now retrieving city number 647
    Now retrieving city number 648
    Now retrieving city number 649
    Now retrieving city number 650
    Now retrieving city number 651
    Now retrieving city number 652
    Now retrieving city number 653
    Now retrieving city number 654
    Now retrieving city number 655
    Now retrieving city number 656
    Now retrieving city number 657
    Now retrieving city number 658
    Now retrieving city number 659
    Now retrieving city number 660
    Now retrieving city number 661
    Now retrieving city number 662
    Now retrieving city number 663
    Now retrieving city number 664
    Now retrieving city number 665
    Now retrieving city number 666
    Now retrieving city number 667
    Now retrieving city number 668
    Now retrieving city number 669
    Now retrieving city number 670
    Now retrieving city number 671
    Now retrieving city number 672
    Now retrieving city number 673
    Now retrieving city number 674
    Now retrieving city number 675
    Now retrieving city number 676
    Now retrieving city number 677
    Now retrieving city number 678
    Now retrieving city number 679
    Now retrieving city number 680
    Now retrieving city number 681
    Now retrieving city number 682
    Now retrieving city number 683
    Now retrieving city number 684
    Now retrieving city number 685
    Now retrieving city number 686
    Now retrieving city number 687
    Now retrieving city number 688
    Now retrieving city number 689
    Now retrieving city number 690
    Now retrieving city number 691
    Now retrieving city number 692
    Now retrieving city number 693
    Now retrieving city number 694
    Now retrieving city number 695
    Now retrieving city number 696
    Now retrieving city number 697
    Now retrieving city number 698
    Now retrieving city number 699
    Now retrieving city number 700
    Now retrieving city number 701
    Now retrieving city number 702
    Now retrieving city number 703
    Now retrieving city number 704
    Now retrieving city number 705
    Now retrieving city number 706
    Now retrieving city number 707
    Now retrieving city number 708
    Now retrieving city number 709
    Now retrieving city number 710
    Now retrieving city number 711
    Now retrieving city number 712
    Now retrieving city number 713
    Now retrieving city number 714
    Now retrieving city number 715
    Now retrieving city number 716
    Now retrieving city number 717
    Now retrieving city number 718
    Now retrieving city number 719
    Now retrieving city number 720
    Now retrieving city number 721
    Now retrieving city number 722
    Now retrieving city number 723
    Now retrieving city number 724
    Now retrieving city number 725
    Now retrieving city number 726
    Now retrieving city number 727
    Now retrieving city number 728
    Now retrieving city number 729
    Now retrieving city number 730
    Now retrieving city number 731
    Now retrieving city number 732
    Now retrieving city number 733
    Now retrieving city number 734
    Now retrieving city number 735
    Now retrieving city number 736
    Now retrieving city number 737
    Now retrieving city number 738
    Now retrieving city number 739
    Now retrieving city number 740
    Now retrieving city number 741
    Now retrieving city number 742
    Now retrieving city number 743
    Now retrieving city number 744
    Now retrieving city number 745
    Now retrieving city number 746
    Now retrieving city number 747
    Now retrieving city number 748
    Now retrieving city number 749
    Now retrieving city number 750
    Now retrieving city number 751
    Now retrieving city number 752
    Now retrieving city number 753
    Now retrieving city number 754
    Now retrieving city number 755
    Now retrieving city number 756
    Now retrieving city number 757
    Now retrieving city number 758
    Now retrieving city number 759
    Now retrieving city number 760
    Now retrieving city number 761
    Now retrieving city number 762
    Now retrieving city number 763
    Now retrieving city number 764
    Now retrieving city number 765
    Now retrieving city number 766
    Now retrieving city number 767
    Now retrieving city number 768
    Now retrieving city number 769
    Now retrieving city number 770
    Now retrieving city number 771
    Now retrieving city number 772
    Now retrieving city number 773
    Now retrieving city number 774
    Now retrieving city number 775
    Now retrieving city number 776
    Now retrieving city number 777
    Now retrieving city number 778
    Now retrieving city number 779
    Now retrieving city number 780
    Now retrieving city number 781
    Now retrieving city number 782
    Now retrieving city number 783
    Now retrieving city number 784
    Now retrieving city number 785
    Now retrieving city number 786
    Now retrieving city number 787
    Now retrieving city number 788
    Now retrieving city number 789
    Now retrieving city number 790
    Now retrieving city number 791
    Now retrieving city number 792
    Now retrieving city number 793
    Now retrieving city number 794
    Now retrieving city number 795
    Now retrieving city number 796
    Now retrieving city number 797
    Now retrieving city number 798
    Now retrieving city number 799
    Now retrieving city number 800
    Now retrieving city number 801
    Now retrieving city number 802
    Now retrieving city number 803
    Now retrieving city number 804
    Now retrieving city number 805
    Now retrieving city number 806
    Now retrieving city number 807
    Now retrieving city number 808
    Now retrieving city number 809
    Now retrieving city number 810
    Now retrieving city number 811
    Now retrieving city number 812
    Now retrieving city number 813
    Now retrieving city number 814
    Now retrieving city number 815
    Now retrieving city number 816
    Now retrieving city number 817
    Now retrieving city number 818
    Now retrieving city number 819
    Now retrieving city number 820
    Now retrieving city number 821
    Now retrieving city number 822
    Now retrieving city number 823
    Now retrieving city number 824
    Now retrieving city number 825
    Now retrieving city number 826
    Now retrieving city number 827
    Now retrieving city number 828
    Now retrieving city number 829
    Now retrieving city number 830
    Now retrieving city number 831
    Now retrieving city number 832
    Now retrieving city number 833
    Now retrieving city number 834
    Now retrieving city number 835
    Now retrieving city number 836
    Now retrieving city number 837
    Now retrieving city number 838
    Now retrieving city number 839
    Now retrieving city number 840
    Now retrieving city number 841
    Now retrieving city number 842
    Now retrieving city number 843
    Now retrieving city number 844
    Now retrieving city number 845
    Now retrieving city number 846
    Now retrieving city number 847
    Now retrieving city number 848
    Now retrieving city number 849
    Now retrieving city number 850
    Now retrieving city number 851
    Now retrieving city number 852
    Now retrieving city number 853
    Now retrieving city number 854
    Now retrieving city number 855
    Now retrieving city number 856
    Now retrieving city number 857
    Now retrieving city number 858
    Now retrieving city number 859
    Now retrieving city number 860
    Now retrieving city number 861
    Now retrieving city number 862
    Now retrieving city number 863
    Now retrieving city number 864
    Now retrieving city number 865
    Now retrieving city number 866
    Now retrieving city number 867
    


```python
# drop cities with NaN and send to CSV
city_final = city_subset.dropna(how='any')
city_final = city_final.sample(500)

city_final.to_csv("City_Weather_Data.csv")
```


```python
# Import pdf image of global map generated in Tableau
from wand.image import Image as WImage
image = WImage(filename='Random_Geo_Coordinates_Map.pdf')
image
```

    Exception ignored in: <bound method Resource.__del__ of <wand.image.Image: (empty)>>
    Traceback (most recent call last):
      File "C:\Users\bmodi\Anaconda3\envs\PythonData\lib\site-packages\wand\resource.py", line 232, in __del__
        self.destroy()
      File "C:\Users\bmodi\Anaconda3\envs\PythonData\lib\site-packages\wand\image.py", line 2767, in destroy
        for i in range(0, len(self.sequence)):
    TypeError: object of type 'NoneType' has no len()
    




![png](output_8_1.png)



## Plot Temperature (F) vs Latitude


```python
city_final.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>City</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>157</th>
      <td>41.280485</td>
      <td>22.267867</td>
      <td>demir kapija</td>
      <td>39.83</td>
      <td>87.0</td>
      <td>8.0</td>
      <td>2.82</td>
    </tr>
    <tr>
      <th>882</th>
      <td>-36.695485</td>
      <td>22.990587</td>
      <td>knysna</td>
      <td>59.00</td>
      <td>93.0</td>
      <td>68.0</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>302</th>
      <td>67.857847</td>
      <td>-61.950600</td>
      <td>pangnirtung</td>
      <td>1.18</td>
      <td>71.0</td>
      <td>56.0</td>
      <td>6.22</td>
    </tr>
    <tr>
      <th>1821</th>
      <td>21.574286</td>
      <td>5.980739</td>
      <td>arlit</td>
      <td>68.36</td>
      <td>26.0</td>
      <td>0.0</td>
      <td>8.34</td>
    </tr>
    <tr>
      <th>1535</th>
      <td>6.392993</td>
      <td>110.907227</td>
      <td>miri</td>
      <td>66.56</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>1.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Build a scatter plot for each data type
plt.scatter(city_final["Latitude"], city_final["Temperature"], edgecolor="none", linewidths=0.5, marker="o", alpha=0.65)
# Incorporate the other graph properties
plt.title("Temperature (F) vs Latitude")
plt.ylabel("Degrees Fehrenheit")
plt.xlabel("Latitude")
plt.grid(linestyle='dotted')
plt.xlim([-95, 95])

# Save the figure
plt.savefig("TempvsLat.png")

plt.show()
```


![png](output_11_0.png)


## Plot Humidity (%) vs Latitude


```python
# Build a scatter plot for each data type
plt.scatter(city_final["Latitude"], city_final["Humidity"], edgecolor="none", linewidths=0.5, marker="o", alpha=0.65)
# Incorporate the other graph properties
plt.title("Humidity (%) vs Latitude")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(linestyle='dotted')
plt.xlim([-95, 95])

# Save the figure
plt.savefig("HumidityvsLat.png")

plt.show()
```


![png](output_13_0.png)


## Cloudiness (%) vs Latitude


```python
# Build a scatter plot for each data type
plt.scatter(city_final["Latitude"], city_final["Cloudiness"], edgecolor="none", linewidths=0.5, marker="o", alpha=0.65)
# Incorporate the other graph properties
plt.title("Cloudiness (%) vs Latitude")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(linestyle='dotted')
plt.xlim([-95, 95])

# Save the figure
plt.savefig("CloudsvsLat.png")

plt.show()
```


![png](output_15_0.png)


## Windspeed (mph) vs Latitude


```python
# Build a scatter plot for each data type
plt.scatter(city_final["Latitude"], city_final["Wind speed"], edgecolor="none", linewidths=0.5, marker="o", alpha=0.65)
# Incorporate the other graph properties
plt.title("Wind speed (mph) vs Latitude")
plt.ylabel("Wind speed (mph)")
plt.xlabel("Latitude")
plt.grid(linestyle='dotted')
plt.xlim([-95, 95])

# Save the figure
plt.savefig("WindvsLat.png")

plt.show()
```


![png](output_17_0.png)

