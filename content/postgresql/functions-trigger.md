[#id](#FUNCTIONS-TRIGGER)

## 9.28. Trigger Functions [#](#FUNCTIONS-TRIGGER)

While many uses of triggers involve user-written trigger functions, PostgreSQL provides a few built-in trigger functions that can be used directly in user-defined triggers. These are summarized in [Table 9.103](functions-trigger#BUILTIN-TRIGGERS-TABLE). (Additional built-in trigger functions exist, which implement foreign key constraints and deferred index constraints. Those are not documented here since users need not use them directly.)

For more information about creating triggers, see [CREATE TRIGGER](sql-createtrigger).

[#id](#BUILTIN-TRIGGERS-TABLE)

**Table 9.103. Built-In Trigger Functions**

<figure class="table-wrapper">
  <table class="table" summary="Built-In Trigger Functions" border="1">
    <colgroup>
      <col />
    </colgroup>
    <thead>
      <tr>
        <th class="func_table_entry">
          <div class="func_signature">Function</div>
          <div>Description</div>
          <div>Example Usage</div>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="func_table_entry">
          <div class="func_signature">
            <a id="id-1.5.8.34.4.2.2.1.1.1.1" class="indexterm"></a>
            <code class="function">suppress_redundant_updates_trigger</code> ( ) →
            <code class="returnvalue">trigger</code>
          </div>
          <div>Suppresses do-nothing update operations. See below for details.</div>
          <div>
            <code class="literal">CREATE TRIGGER ... suppress_redundant_updates_trigger()</code>
          </div>
        </td>
      </tr>
      <tr>
        <td class="func_table_entry">
          <div class="func_signature">
            <a id="id-1.5.8.34.4.2.2.2.1.1.1" class="indexterm"></a>
            <code class="function">tsvector_update_trigger</code> ( ) →
            <code class="returnvalue">trigger</code>
          </div>
          <div>
            Automatically updates a <code class="type">tsvector</code> column from associated
            plain-text document column(s). The text search configuration to use is specified by name
            as a trigger argument. See
            <a
              class="xref"
              href="textsearch-features.html#TEXTSEARCH-UPDATE-TRIGGERS"
              title="12.4.3.&nbsp;Triggers for Automatic Updates">Section&nbsp;12.4.3</a>
            for details.
          </div>
          <div>
            <code class="literal">CREATE TRIGGER ... tsvector_update_trigger(tsvcol, 'pg_catalog.swedish', title,
              body)</code>
          </div>
        </td>
      </tr>
      <tr>
        <td class="func_table_entry">
          <div class="func_signature">
            <a id="id-1.5.8.34.4.2.2.3.1.1.1" class="indexterm"></a>
            <code class="function">tsvector_update_trigger_column</code> ( ) →
            <code class="returnvalue">trigger</code>
          </div>
          <div>
            Automatically updates a <code class="type">tsvector</code> column from associated
            plain-text document column(s). The text search configuration to use is taken from a
            <code class="type">regconfig</code> column of the table. See
            <a
              class="xref"
              href="textsearch-features.html#TEXTSEARCH-UPDATE-TRIGGERS"
              title="12.4.3.&nbsp;Triggers for Automatic Updates">Section&nbsp;12.4.3</a>
            for details.
          </div>
          <div>
            <code class="literal">CREATE TRIGGER ... tsvector_update_trigger_column(tsvcol, tsconfigcol, title,
              body)</code>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</figure>

The `suppress_redundant_updates_trigger` function, when applied as a row-level `BEFORE UPDATE` trigger, will prevent any update that does not actually change the data in the row from taking place. This overrides the normal behavior which always performs a physical row update regardless of whether or not the data has changed. (This normal behavior makes updates run faster, since no checking is required, and is also useful in certain cases.)

Ideally, you should avoid running updates that don't actually change the data in the record. Redundant updates can cost considerable unnecessary time, especially if there are lots of indexes to alter, and space in dead rows that will eventually have to be vacuumed. However, detecting such situations in client code is not always easy, or even possible, and writing expressions to detect them can be error-prone. An alternative is to use `suppress_redundant_updates_trigger`, which will skip updates that don't change the data. You should use this with care, however. The trigger takes a small but non-trivial time for each record, so if most of the records affected by updates do actually change, use of this trigger will make updates run slower on average.

The `suppress_redundant_updates_trigger` function can be added to a table like this:

```

CREATE TRIGGER z_min_update
BEFORE UPDATE ON tablename
FOR EACH ROW EXECUTE FUNCTION suppress_redundant_updates_trigger();
```

In most cases, you need to fire this trigger last for each row, so that it does not override other triggers that might wish to alter the row. Bearing in mind that triggers fire in name order, you would therefore choose a trigger name that comes after the name of any other trigger you might have on the table. (Hence the “z” prefix in the example.)
