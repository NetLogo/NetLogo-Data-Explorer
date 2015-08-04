Warning! Never run `clear-all`! Use `clear-turtles`, `clear-patches`, and so forth instead.

Getting started
---

1. Open up `NetLogo Data Explorer.nlogo`. Click **setup** and then **go**. There should now be a **Notebook** tab.
2. Hit **3D** on the NetLogo view to pop out a 3D view in a seperate window.
3. Switch to the **Notebook** tab. You should see a code editor that says:

        ; To get rid of headers of files, use `drop`. For instance,
        ; set data drop 6 csv:from-file user-file
        ; will get rid of the headers of BehaviorSpace files
        set data csv:from-file user-file
        print-value (word "Number of rows: " (length data - 1))
        print-table take 15 data

    and a **Run** button. `user-file` is a built-in NetLogo primitive that opens up a open-file dialog and reports whatever the user chooses. `csv:from-file` reads in a CSV file as a list of lists, where each list is a row. `take 20 <list>` simply reports the first 20 items of a list. `print-table` prints out a list of lists in tabular format in an output window that will be created when you hit **Run**. So, this code reads in a CSV file, stores it in the `data` variable, and then prints out the first 20 rows of it a nicely formatted way.

4. Go ahead and hit **Run**. Select a CSV file and hit **Open**. The first 20 lines of the CSV file should be printed, with rows numbered and columns lined up together.

5. If you chose a BehaviorSpace file, the first 6 lines of the file will be meta-data that we don't care about. To get rid of it, write `drop 6` in front of the `csv:from-file user-file`. Hit **Run** again. The data should now be trimmed down to the part of the file that actually contains the run data. In this way, you can easily tweak how your data is to be imported and re-import it quickly.

6. After hitting **Run**, an output box will have appeared and then another code editor below that. In the new code editor write:

        create-turtles-from-data data task [
          set-x get "one-column"
          set-y get "another-column"
        ]

    where you should replace `one-column` and `another-column` with actual column names from your data. Hit **Run**. A bunch of turtles (one for each row) should be created!

7. Hit **Interact** in the view. When you mouse over turtles, you can now see their coordinates.


Primitives
---

### Notebook

#### `print-value <anything>`

Prints out its argument in the output window below the current editor.

#### `print-table <list-of-lists>`

Prints out the given tabular data in the output window, with rows numbered and columns lined up.

### Data

#### `csv:from-file <string>`

Reports the contents of the given CSV file as a list of lists, where each list is a row from the file.

#### `select <list-of-strings> <list-of-lists>`

Creates a new table of data that consists of only the columns specified by `<list-of-strings>`.  For example, `select ["density" "percent-burned"] data` will select out the `"density"` and the `"percent-burned"` columns from data.

`query <reporter-task> <list-of-strings> <list-of-lists>`

Reports just the rows for which the `<reporter-task>` reports true, given the specified columns as arguments. For example, `query task [?1 > ?2] ["density" "percent-burned"] data` gets the rows where the `"density"` is greater than the `"percent-burned"`.

#### `sorted-on <string> <list-of-lists>`

Reports the tabular data with rows sorted by the column specified by `<string>`. So, `sorted-on "density" data` will sort data based on the `"density"` column.

#### `group-by <list-of-strings> <list-of-lists>`

Groups the values of rows together that share the same columns specified by `<list-of-strings>`. For instance, `group-by ["density"] data` will group all the rows together that have the same `"density"`. The rows of the newly created table will be lists of lists, where each value is the list of all the values of the grouped rows. It's kind of confusing, but very useful, so play around with it.

#### `tanspose <list-of-lists>`

Makes the columns the rows and the rows the columns!

#### `get-col <string> <list-of-lists>`

Reports the given column as a list. Useful for finding, for instance, the max of columns: `max get-col "percent-burned" data`.

#### `set-col <string> <list> <list-of-lists>`

Reports a new table with the values of column `<string>` replaced with `<list>`.

### Turtles

#### `create-turtles-from-data <list-of-lists> <task>`

Creates one turtle per row of the given data. Each turtle is assigned that row (see `get` below). The turtles run the given task after creation.

#### `get <string>`

Reports the turtle's value for the given column name.

#### `set-x <number>`, `set-y <number>`, `set-xy <number>`

Sets turtles' coordinates to the given number. Changes the boundaries of the world so that all turtles fit in with their coordinates. This makes it so that you don't have to worry about data values fitting in the world's coordinate system, but it does mean things like `forward` don't work. If you want to disable this feature and just work with NetLogo's coordinates, turn **go** off and just use normal turtle primitives.

### Utility

#### `heat <number>`

Given a number between 0 and 1, reports a color. Modeled after the [cubehelix](https://www.mrao.cam.ac.uk/~dag/CUBEHELIX/) popular in astronomy. A bit easier than `scale-color` in my opinion.

#### `drop <number> <list>`

Reports a copy of `<list>` with the first `<number>` items removed. Identical to `sublist <list> <number> (length <list>)`.

#### `take <number> <list>`

Reports the first `<number>` items of `<list>`. Identical to `sublist <list> 0 <number>`.

#### `tail <number> <list>`

Reports the last `<number>` items of `<list>`. Identical to `sublist <list> (length <list> - <number>) (length <list>)`.
