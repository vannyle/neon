[#id](#ISN)

## F.21. isn — data types for international standard numbers (ISBN, EAN, UPC, etc.) [#](#ISN)

- [F.21.1. Data Types](isn#ISN-DATA-TYPES)
- [F.21.2. Casts](isn#ISN-CASTS)
- [F.21.3. Functions and Operators](isn#ISN-FUNCS-OPS)
- [F.21.4. Examples](isn#ISN-EXAMPLES)
- [F.21.5. Bibliography](isn#ISN-BIBLIOGRAPHY)
- [F.21.6. Author](isn#ISN-AUTHOR)

The `isn` module provides data types for the following international product numbering standards: EAN13, UPC, ISBN (books), ISMN (music), and ISSN (serials). Numbers are validated on input according to a hard-coded list of prefixes; this list of prefixes is also used to hyphenate numbers on output. Since new prefixes are assigned from time to time, the list of prefixes may be out of date. It is hoped that a future version of this module will obtain the prefix list from one or more tables that can be easily updated by users as needed; however, at present, the list can only be updated by modifying the source code and recompiling. Alternatively, prefix validation and hyphenation support may be dropped from a future version of this module.

This module is considered “trusted”, that is, it can be installed by non-superusers who have `CREATE` privilege on the current database.

[#id](#ISN-DATA-TYPES)

### F.21.1. Data Types [#](#ISN-DATA-TYPES)

[Table F.11](isn#ISN-DATATYPES) shows the data types provided by the `isn` module.

[#id](#ISN-DATATYPES)

**Table F.11. `isn` Data Types**

| Data Type | Description                                                                           |
| --------- | ------------------------------------------------------------------------------------- |
| `EAN13`   | European Article Numbers, always displayed in the EAN13 display format                |
| `ISBN13`  | International Standard Book Numbers to be displayed in the new EAN13 display format   |
| `ISMN13`  | International Standard Music Numbers to be displayed in the new EAN13 display format  |
| `ISSN13`  | International Standard Serial Numbers to be displayed in the new EAN13 display format |
| `ISBN`    | International Standard Book Numbers to be displayed in the old short display format   |
| `ISMN`    | International Standard Music Numbers to be displayed in the old short display format  |
| `ISSN`    | International Standard Serial Numbers to be displayed in the old short display format |
| `UPC`     | Universal Product Codes                                                               |

Some notes:

1. ISBN13, ISMN13, ISSN13 numbers are all EAN13 numbers.

2. EAN13 numbers aren't always ISBN13, ISMN13 or ISSN13 (some are).

3. Some ISBN13 numbers can be displayed as ISBN.

4. Some ISMN13 numbers can be displayed as ISMN.

5. Some ISSN13 numbers can be displayed as ISSN.

6. UPC numbers are a subset of the EAN13 numbers (they are basically EAN13 without the first `0` digit).

7. All UPC, ISBN, ISMN and ISSN numbers can be represented as EAN13 numbers.

Internally, all these types use the same representation (a 64-bit integer), and all are interchangeable. Multiple types are provided to control display formatting and to permit tighter validity checking of input that is supposed to denote one particular type of number.

The `ISBN`, `ISMN`, and `ISSN` types will display the short version of the number (ISxN 10) whenever it's possible, and will show ISxN 13 format for numbers that do not fit in the short version. The `EAN13`, `ISBN13`, `ISMN13` and `ISSN13` types will always display the long version of the ISxN (EAN13).

[#id](#ISN-CASTS)

### F.21.2. Casts [#](#ISN-CASTS)

The `isn` module provides the following pairs of type casts:

- `ISBN13 <=> EAN13`

- `ISMN13 <=> EAN13`

- `ISSN13 <=> EAN13`

- `ISBN <=> EAN13`

- `ISMN <=> EAN13`

- `ISSN <=> EAN13`

- `UPC <=> EAN13`

- `ISBN <=> ISBN13`

- `ISMN <=> ISMN13`

- `ISSN <=> ISSN13`

When casting from `EAN13` to another type, there is a run-time check that the value is within the domain of the other type, and an error is thrown if not. The other casts are simply relabelings that will always succeed.

[#id](#ISN-FUNCS-OPS)

### F.21.3. Functions and Operators [#](#ISN-FUNCS-OPS)

The `isn` module provides the standard comparison operators, plus B-tree and hash indexing support for all these data types. In addition there are several specialized functions; shown in [Table F.12](isn#ISN-FUNCTIONS). In this table, `isn` means any one of the module's data types.

[#id](#ISN-FUNCTIONS)

**Table F.12. `isn` Functions**

<figure class="table-wrapper">
<table class="table" summary="isn Functions" border="1">
  <colgroup>
    <col />
  </colgroup>
  <thead>
    <tr>
      <th class="func_table_entry">
        <div class="func_signature">Function</div>
        <div>Description</div>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.11.7.31.7.3.2.2.1.1.1.1" class="indexterm"></a>
          <code class="function">isn_weak</code> ( <code class="type">boolean</code> ) →
          <code class="returnvalue">boolean</code>
        </div>
        <div>Sets the weak input mode, and returns new setting.</div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">isn_weak</code> () → <code class="returnvalue">boolean</code>
        </div>
        <div>Returns the current status of the weak mode.</div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.11.7.31.7.3.2.2.3.1.1.1" class="indexterm"></a>
          <code class="function">make_valid</code> ( <code class="type">isn</code> ) →
          <code class="returnvalue">isn</code>
        </div>
        <div>Validates an invalid number (clears the invalid flag).</div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.11.7.31.7.3.2.2.4.1.1.1" class="indexterm"></a>
          <code class="function">is_valid</code> ( <code class="type">isn</code> ) →
          <code class="returnvalue">boolean</code>
        </div>
        <div>Checks for the presence of the invalid flag.</div>
      </td>
    </tr>
  </tbody>
</table>
</figure>

_Weak_ mode is used to be able to insert invalid data into a table. Invalid means the check digit is wrong, not that there are missing numbers.

Why would you want to use the weak mode? Well, it could be that you have a huge collection of ISBN numbers, and that there are so many of them that for weird reasons some have the wrong check digit (perhaps the numbers were scanned from a printed list and the OCR got the numbers wrong, perhaps the numbers were manually captured... who knows). Anyway, the point is you might want to clean the mess up, but you still want to be able to have all the numbers in your database and maybe use an external tool to locate the invalid numbers in the database so you can verify the information and validate it more easily; so for example you'd want to select all the invalid numbers in the table.

When you insert invalid numbers in a table using the weak mode, the number will be inserted with the corrected check digit, but it will be displayed with an exclamation mark (`!`) at the end, for example `0-11-000322-5!`. This invalid marker can be checked with the `is_valid` function and cleared with the `make_valid` function.

You can also force the insertion of invalid numbers even when not in the weak mode, by appending the `!` character at the end of the number.

Another special feature is that during input, you can write `?` in place of the check digit, and the correct check digit will be inserted automatically.

[#id](#ISN-EXAMPLES)

### F.21.4. Examples [#](#ISN-EXAMPLES)

```
--Using the types directly:
SELECT isbn('978-0-393-04002-9');
SELECT isbn13('0901690546');
SELECT issn('1436-4522');

--Casting types:
-- note that you can only cast from ean13 to another type when the
-- number would be valid in the realm of the target type;
-- thus, the following will NOT work: select isbn(ean13('0220356483481'));
-- but these will:
SELECT upc(ean13('0220356483481'));
SELECT ean13(upc('220356483481'));

--Create a table with a single column to hold ISBN numbers:
CREATE TABLE test (id isbn);
INSERT INTO test VALUES('9780393040029');

--Automatically calculate check digits (observe the '?'):
INSERT INTO test VALUES('220500896?');
INSERT INTO test VALUES('978055215372?');

SELECT issn('3251231?');
SELECT ismn('979047213542?');

--Using the weak mode:
SELECT isn_weak(true);
INSERT INTO test VALUES('978-0-11-000533-4');
INSERT INTO test VALUES('9780141219307');
INSERT INTO test VALUES('2-205-00876-X');
SELECT isn_weak(false);

SELECT id FROM test WHERE NOT is_valid(id);
UPDATE test SET id = make_valid(id) WHERE id = '2-205-00876-X!';

SELECT * FROM test;

SELECT isbn13(id) FROM test;
```

[#id](#ISN-BIBLIOGRAPHY)

### F.21.5. Bibliography [#](#ISN-BIBLIOGRAPHY)

The information to implement this module was collected from several sites, including:

- [https://www.isbn-international.org/](https://www.isbn-international.org/)

- [https://www.issn.org/](https://www.issn.org/)

- [https://www.ismn-international.org/](https://www.ismn-international.org/)

- [https://www.wikipedia.org/](https://www.wikipedia.org/)

The prefixes used for hyphenation were also compiled from:

- [https://www.gs1.org/standards/id-keys](https://www.gs1.org/standards/id-keys)

- [https://en.wikipedia.org/wiki/List_of_ISBN_identifier_groups](https://en.wikipedia.org/wiki/List_of_ISBN_identifier_groups)

- [https://www.isbn-international.org/content/isbn-users-manual](https://www.isbn-international.org/content/isbn-users-manual)

- [https://en.wikipedia.org/wiki/International_Standard_Music_Number](https://en.wikipedia.org/wiki/International_Standard_Music_Number)

- [https://www.ismn-international.org/ranges.html](https://www.ismn-international.org/ranges.html)

Care was taken during the creation of the algorithms and they were meticulously verified against the suggested algorithms in the official ISBN, ISMN, ISSN User Manuals.

[#id](#ISN-AUTHOR)

### F.21.6. Author [#](#ISN-AUTHOR)

Germán Méndez Bravo (Kronuz), 2004–2006

This module was inspired by Garrett A. Wollman's `isbn_issn` code.
