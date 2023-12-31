[#id](#FUNCTIONS-GEOMETRY)

## 9.11. Geometric Functions and Operators [#](#FUNCTIONS-GEOMETRY)

The geometric types `point`, `box`, `lseg`, `line`, `path`, `polygon`, and `circle` have a large set of native support functions and operators, shown in [Table 9.36](functions-geometry#FUNCTIONS-GEOMETRY-OP-TABLE), [Table 9.37](functions-geometry#FUNCTIONS-GEOMETRY-FUNC-TABLE), and [Table 9.38](functions-geometry#FUNCTIONS-GEOMETRY-CONV-TABLE).

[#id](#FUNCTIONS-GEOMETRY-OP-TABLE)

**Table 9.36. Geometric Operators**

<figure class="table-wrapper">
<table class="table" summary="Geometric Operators" border="1">
  <colgroup>
    <col />
  </colgroup>
  <thead>
    <tr>
      <th class="func_table_entry">
        <div class="func_signature">Operator</div>
        <div>Description</div>
        <div>Example(s)</div>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em> <code class="literal">+</code>
          <code class="type">point</code> →
          <code class="returnvalue"><em class="replaceable"><code>geometric_type</code></em></code>
        </div>
        <div>
          Adds the coordinates of the second <code class="type">point</code> to those of each point
          of the first argument, thus performing translation. Available for
          <code class="type">point</code>, <code class="type">box</code>,
          <code class="type">path</code>, <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(1,1),(0,0)' + point '(2,0)'</code>
          → <code class="returnvalue">(3,1),(2,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="type">path</code> <code class="literal">+</code>
          <code class="type">path</code> → <code class="returnvalue">path</code>
        </div>
        <div>Concatenates two open paths (returns NULL if either path is closed).</div>
        <div>
          <code class="literal">path '[(0,0),(1,1)]' + path '[(2,2),(3,3),(4,4)]'</code>
          → <code class="returnvalue">[(0,0),(1,1),(2,2),(3,3),(4,4)]</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em> <code class="literal">-</code>
          <code class="type">point</code> →
          <code class="returnvalue"><em class="replaceable"><code>geometric_type</code></em></code>
        </div>
        <div>
          Subtracts the coordinates of the second <code class="type">point</code> from those of each
          point of the first argument, thus performing translation. Available for
          <code class="type">point</code>, <code class="type">box</code>,
          <code class="type">path</code>, <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(1,1),(0,0)' - point '(2,0)'</code>
          → <code class="returnvalue">(-1,1),(-2,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em> <code class="literal">*</code>
          <code class="type">point</code> →
          <code class="returnvalue"><em class="replaceable"><code>geometric_type</code></em></code>
        </div>
        <div>
          Multiplies each point of the first argument by the second
          <code class="type">point</code> (treating a point as being a complex number represented by
          real and imaginary parts, and performing standard complex multiplication). If one
          interprets the second <code class="type">point</code> as a vector, this is equivalent to
          scaling the object's size and distance from the origin by the length of the vector, and
          rotating it counterclockwise around the origin by the vector's angle from the
          <em class="replaceable"><code>x</code></em> axis. Available for
          <code class="type">point</code>, <code class="type">box</code>,<a
            href="#ftn.FUNCTIONS-GEOMETRY-ROTATION-FN"
            class="footnote"><sup class="footnote" id="FUNCTIONS-GEOMETRY-ROTATION-FN">[a]</sup></a>
          <code class="type">path</code>, <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">path '((0,0),(1,0),(1,1))' *point '(3.0,0)'</code>
          → <code class="returnvalue">((0,0),(3,0),(3,3))</code>
        </div>
        <div>
          <code class="literal">path '((0,0),(1,0),(1,1))'* point(cosd(45), sind(45))</code>
          →
          <code class="returnvalue">((0,0),​(0.7071067811865475,0.7071067811865475),​(0,1.414213562373095))</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em> <code class="literal">/</code>
          <code class="type">point</code> →
          <code class="returnvalue"><em class="replaceable"><code>geometric_type</code></em></code>
        </div>
        <div>
          Divides each point of the first argument by the second
          <code class="type">point</code> (treating a point as being a complex number represented by
          real and imaginary parts, and performing standard complex division). If one interprets the
          second <code class="type">point</code> as a vector, this is equivalent to scaling the
          object's size and distance from the origin down by the length of the vector, and rotating
          it clockwise around the origin by the vector's angle from the
          <em class="replaceable"><code>x</code></em> axis. Available for
          <code class="type">point</code>, <code class="type">box</code>,<a
            href="functions-geometry.html#ftn.FUNCTIONS-GEOMETRY-ROTATION-FN"
            class="footnoteref"><sup class="footnoteref">[a]</sup></a>
          <code class="type">path</code>, <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">path '((0,0),(1,0),(1,1))' / point '(2.0,0)'</code>
          → <code class="returnvalue">((0,0),(0.5,0),(0.5,0.5))</code>
        </div>
        <div>
          <code class="literal">path '((0,0),(1,0),(1,1))' / point(cosd(45), sind(45))</code>
          →
          <code class="returnvalue">((0,0),​(0.7071067811865476,-0.7071067811865476),​(1.4142135623730951,0))</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="literal">@-@</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">double precision</code>
        </div>
        <div>
          Computes the total length. Available for <code class="type">lseg</code>,
          <code class="type">path</code>.
        </div>
        <div>
          <code class="literal">@-@ path '[(0,0),(1,0),(1,1)]'</code>
          → <code class="returnvalue">2</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="literal">@@</code> <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">point</code>
        </div>
        <div>
          Computes the center point. Available for <code class="type">box</code>,
          <code class="type">lseg</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">@@ box '(2,2),(0,0)'</code>
          → <code class="returnvalue">(1,1)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="literal">#</code> <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">integer</code>
        </div>
        <div>
          Returns the number of points. Available for <code class="type">path</code>,
          <code class="type">polygon</code>.
        </div>
        <div>
          <code class="literal"># path '((1,0),(0,1),(-1,0))'</code>
          → <code class="returnvalue">3</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em> <code class="literal">#</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">point</code>
        </div>
        <div>
          Computes the point of intersection, or NULL if there is none. Available for
          <code class="type">lseg</code>, <code class="type">line</code>.
        </div>
        <div>
          <code class="literal">lseg '[(0,0),(1,1)]' # lseg '[(1,0),(0,1)]'</code>
          → <code class="returnvalue">(0.5,0.5)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="type">box</code> <code class="literal">#</code>
          <code class="type">box</code> → <code class="returnvalue">box</code>
        </div>
        <div>Computes the intersection of two boxes, or NULL if there is none.</div>
        <div>
          <code class="literal">box '(2,2),(-1,-1)' # box '(1,1),(-2,-2)'</code>
          → <code class="returnvalue">(1,1),(-1,-1)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em> <code class="literal">##</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">point</code>
        </div>
        <div>
          Computes the closest point to the first object on the second object. Available for these
          pairs of types: (<code class="type">point</code>, <code class="type">box</code>), (<code
            class="type">point</code>, <code class="type">lseg</code>), (<code class="type">point</code>,
          <code class="type">line</code>), (<code class="type">lseg</code>,
          <code class="type">box</code>), (<code class="type">lseg</code>,
          <code class="type">lseg</code>), (<code class="type">line</code>,
          <code class="type">lseg</code>).
        </div>
        <div>
          <code class="literal">point '(0,0)' ## lseg '[(2,0),(0,2)]'</code>
          → <code class="returnvalue">(1,1)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&lt;-&gt;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">double precision</code>
        </div>
        <div>
          Computes the distance between the objects. Available for all seven geometric types, for
          all combinations of <code class="type">point</code> with another geometric type, and for
          these additional pairs of types: (<code class="type">box</code>,
          <code class="type">lseg</code>), (<code class="type">lseg</code>,
          <code class="type">line</code>), (<code class="type">polygon</code>,
          <code class="type">circle</code>) (and the commutator cases).
        </div>
        <div>
          <code class="literal">circle '&lt;(0,0),1&gt;' &lt;-&gt; circle '&lt;(5,0),1&gt;'</code>
          → <code class="returnvalue">3</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">@&gt;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Does first object contain second? Available for these pairs of types: (<code
            class="literal">box</code>, <code class="literal">point</code>), (<code class="literal">box</code>,
          <code class="literal">box</code>), (<code class="literal">path</code>,
          <code class="literal">point</code>), (<code class="literal">polygon</code>,
          <code class="literal">point</code>), (<code class="literal">polygon</code>,
          <code class="literal">polygon</code>), (<code class="literal">circle</code>,
          <code class="literal">point</code>), (<code class="literal">circle</code>,
          <code class="literal">circle</code>).
        </div>
        <div>
          <code class="literal">circle '&lt;(0,0),2&gt;' @&gt; point '(1,1)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&lt;@</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Is first object contained in or on second? Available for these pairs of types: (<code
            class="literal">point</code>, <code class="literal">box</code>), (<code class="literal">point</code>,
          <code class="literal">lseg</code>), (<code class="literal">point</code>,
          <code class="literal">line</code>), (<code class="literal">point</code>,
          <code class="literal">path</code>), (<code class="literal">point</code>,
          <code class="literal">polygon</code>), (<code class="literal">point</code>,
          <code class="literal">circle</code>), (<code class="literal">box</code>,
          <code class="literal">box</code>), (<code class="literal">lseg</code>,
          <code class="literal">box</code>), (<code class="literal">lseg</code>,
          <code class="literal">line</code>), (<code class="literal">polygon</code>,
          <code class="literal">polygon</code>), (<code class="literal">circle</code>,
          <code class="literal">circle</code>).
        </div>
        <div>
          <code class="literal">point '(1,1)' &lt;@ circle '&lt;(0,0),2&gt;'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&amp;&amp;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Do these objects overlap? (One point in common makes this true.) Available for
          <code class="type">box</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(1,1),(0,0)' &amp;&amp; box '(2,2),(0,0)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&lt;&lt;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Is first object strictly left of second? Available for <code class="type">point</code>,
          <code class="type">box</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">circle '&lt;(0,0),1&gt;' &lt;&lt; circle '&lt;(5,0),1&gt;'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&gt;&gt;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Is first object strictly right of second? Available for <code class="type">point</code>,
          <code class="type">box</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">circle '&lt;(5,0),1&gt;' &gt;&gt; circle '&lt;(0,0),1&gt;'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&amp;&lt;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Does first object not extend to the right of second? Available for
          <code class="type">box</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(1,1),(0,0)' &amp;&lt; box '(2,2),(0,0)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&amp;&gt;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Does first object not extend to the left of second? Available for
          <code class="type">box</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(3,3),(0,0)' &amp;&gt; box '(2,2),(0,0)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&lt;&lt;|</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Is first object strictly below second? Available for <code class="type">point</code>,
          <code class="type">box</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(3,3),(0,0)' &lt;&lt;| box '(5,5),(3,4)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">|&gt;&gt;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Is first object strictly above second? Available for <code class="type">point</code>,
          <code class="type">box</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(5,5),(3,4)' |&gt;&gt; box '(3,3),(0,0)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">&amp;&lt;|</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Does first object not extend above second? Available for <code class="type">box</code>,
          <code class="type">polygon</code>, <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(1,1),(0,0)' &amp;&lt;| box '(2,2),(0,0)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em>
          <code class="literal">|&amp;&gt;</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Does first object not extend below second? Available for <code class="type">box</code>,
          <code class="type">polygon</code>, <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">box '(3,3),(0,0)' |&amp;&gt; box '(2,2),(0,0)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="type">box</code> <code class="literal">&lt;^</code>
          <code class="type">box</code> → <code class="returnvalue">boolean</code>
        </div>
        <div>Is first object below second (allows edges to touch)?</div>
        <div>
          <code class="literal">box '((1,1),(0,0))' &lt;^ box '((2,2),(1,1))'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="type">box</code> <code class="literal">&gt;^</code>
          <code class="type">box</code> → <code class="returnvalue">boolean</code>
        </div>
        <div>Is first object above second (allows edges to touch)?</div>
        <div>
          <code class="literal">box '((2,2),(1,1))' &gt;^ box '((1,1),(0,0))'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em> <code class="literal">?#</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Do these objects intersect? Available for these pairs of types: (<code class="type">box</code>, <code class="type">box</code>), (<code class="type">lseg</code>,
          <code class="type">box</code>), (<code class="type">lseg</code>,
          <code class="type">lseg</code>), (<code class="type">lseg</code>,
          <code class="type">line</code>), (<code class="type">line</code>,
          <code class="type">box</code>), (<code class="type">line</code>,
          <code class="type">line</code>), (<code class="type">path</code>,
          <code class="type">path</code>).
        </div>
        <div>
          <code class="literal">lseg '[(-1,0),(1,0)]' ?# box '(2,2),(-2,-2)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="literal">?-</code> <code class="type">line</code> →
          <code class="returnvalue">boolean</code>
        </div>
        <div class="func_signature">
          <code class="literal">?-</code> <code class="type">lseg</code> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>Is line horizontal?</div>
        <div>
          <code class="literal">?- lseg '[(-1,0),(1,0)]'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="type">point</code> <code class="literal">?-</code>
          <code class="type">point</code> → <code class="returnvalue">boolean</code>
        </div>
        <div>Are points horizontally aligned (that is, have same y coordinate)?</div>
        <div>
          <code class="literal">point '(1,0)' ?- point '(0,0)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="literal">?|</code> <code class="type">line</code> →
          <code class="returnvalue">boolean</code>
        </div>
        <div class="func_signature">
          <code class="literal">?|</code> <code class="type">lseg</code> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>Is line vertical?</div>
        <div>
          <code class="literal">?| lseg '[(-1,0),(1,0)]'</code>
          → <code class="returnvalue">f</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="type">point</code> <code class="literal">?|</code>
          <code class="type">point</code> → <code class="returnvalue">boolean</code>
        </div>
        <div>Are points vertically aligned (that is, have same x coordinate)?</div>
        <div>
          <code class="literal">point '(0,1)' ?| point '(0,0)'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="type">line</code> <code class="literal">?-|</code>
          <code class="type">line</code> → <code class="returnvalue">boolean</code>
        </div>
        <div class="func_signature">
          <code class="type">lseg</code> <code class="literal">?-|</code>
          <code class="type">lseg</code> → <code class="returnvalue">boolean</code>
        </div>
        <div>Are lines perpendicular?</div>
        <div>
          <code class="literal">lseg '[(0,0),(0,1)]' ?-| lseg '[(0,0),(1,0)]'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="type">line</code> <code class="literal">?||</code>
          <code class="type">line</code> → <code class="returnvalue">boolean</code>
        </div>
        <div class="func_signature">
          <code class="type">lseg</code> <code class="literal">?||</code>
          <code class="type">lseg</code> → <code class="returnvalue">boolean</code>
        </div>
        <div>Are lines parallel?</div>
        <div>
          <code class="literal">lseg '[(-1,0),(1,0)]' ?|| lseg '[(-1,2),(1,2)]'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <em class="replaceable"><code>geometric_type</code></em> <code class="literal">~=</code>
          <em class="replaceable"><code>geometric_type</code></em> →
          <code class="returnvalue">boolean</code>
        </div>
        <div>
          Are these objects the same? Available for <code class="type">point</code>,
          <code class="type">box</code>, <code class="type">polygon</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">polygon '((0,0),(1,1))' ~= polygon '((1,1),(0,0))'</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
  </tbody>
  <tbody class="footnotes">
    <tr>
      <td colspan="1">
        <div id="ftn.FUNCTIONS-GEOMETRY-ROTATION-FN" class="footnote">
          <div>
            <a href="#FUNCTIONS-GEOMETRY-ROTATION-FN" class="para"><sup class="para">[a] </sup></a><span class="quote">“<span class="quote">Rotating</span>”</span> a box with these
            operators only moves its corner points: the box is still considered to have sides
            parallel to the axes. Hence the box's size is not preserved, as a true rotation would
            do.
          </div>
        </div>
      </td>
    </tr>
  </tbody>
</table>
</figure>

### Caution

Note that the “same as” operator, `~=`, represents the usual notion of equality for the `point`, `box`, `polygon`, and `circle` types. Some of the geometric types also have an `=` operator, but `=` compares for equal _areas_ only. The other scalar comparison operators (`<=` and so on), where available for these types, likewise compare areas.

### Note

Before PostgreSQL 14, the point is strictly below/above comparison operators `point` `<<|` `point` and `point` `|>>` `point` were respectively called `<^` and `>^`. These names are still available, but are deprecated and will eventually be removed.

[#id](#FUNCTIONS-GEOMETRY-FUNC-TABLE)

**Table 9.37. Geometric Functions**

<figure class="table-wrapper">
<table class="table" summary="Geometric Functions" border="1">
  <colgroup>
    <col />
  </colgroup>
  <thead>
    <tr>
      <th class="func_table_entry">
        <div class="func_signature">Function</div>
        <div>Description</div>
        <div>Example(s)</div>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.1.1.1.1" class="indexterm"></a>
          <code class="function">area</code> (
          <em class="replaceable"><code>geometric_type</code></em> ) →
          <code class="returnvalue">double precision</code>
        </div>
        <div>
          Computes area. Available for <code class="type">box</code>,
          <code class="type">path</code>, <code class="type">circle</code>. A
          <code class="type">path</code> input must be closed, else NULL is returned. Also, if the
          <code class="type">path</code> is self-intersecting, the result may be meaningless.
        </div>
        <div>
          <code class="literal">area(box '(2,2),(0,0)')</code>
          → <code class="returnvalue">4</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.2.1.1.1" class="indexterm"></a>
          <code class="function">center</code> (
          <em class="replaceable"><code>geometric_type</code></em> ) →
          <code class="returnvalue">point</code>
        </div>
        <div>
          Computes center point. Available for <code class="type">box</code>,
          <code class="type">circle</code>.
        </div>
        <div>
          <code class="literal">center(box '(1,2),(0,0)')</code>
          → <code class="returnvalue">(0.5,1)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.3.1.1.1" class="indexterm"></a>
          <code class="function">diagonal</code> ( <code class="type">box</code> ) →
          <code class="returnvalue">lseg</code>
        </div>
        <div>
          Extracts box's diagonal as a line segment (same as
          <code class="function">lseg(box)</code>).
        </div>
        <div>
          <code class="literal">diagonal(box '(1,2),(0,0)')</code>
          → <code class="returnvalue">[(1,2),(0,0)]</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.4.1.1.1" class="indexterm"></a>
          <code class="function">diameter</code> ( <code class="type">circle</code> ) →
          <code class="returnvalue">double precision</code>
        </div>
        <div>Computes diameter of circle.</div>
        <div>
          <code class="literal">diameter(circle '&lt;(0,0),2&gt;')</code>
          → <code class="returnvalue">4</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.5.1.1.1" class="indexterm"></a>
          <code class="function">height</code> ( <code class="type">box</code> ) →
          <code class="returnvalue">double precision</code>
        </div>
        <div>Computes vertical size of box.</div>
        <div>
          <code class="literal">height(box '(1,2),(0,0)')</code>
          → <code class="returnvalue">2</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.6.1.1.1" class="indexterm"></a>
          <code class="function">isclosed</code> ( <code class="type">path</code> ) →
          <code class="returnvalue">boolean</code>
        </div>
        <div>Is path closed?</div>
        <div>
          <code class="literal">isclosed(path '((0,0),(1,1),(2,0))')</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.7.1.1.1" class="indexterm"></a>
          <code class="function">isopen</code> ( <code class="type">path</code> ) →
          <code class="returnvalue">boolean</code>
        </div>
        <div>Is path open?</div>
        <div>
          <code class="literal">isopen(path '[(0,0),(1,1),(2,0)]')</code>
          → <code class="returnvalue">t</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.8.1.1.1" class="indexterm"></a>
          <code class="function">length</code> (
          <em class="replaceable"><code>geometric_type</code></em> ) →
          <code class="returnvalue">double precision</code>
        </div>
        <div>
          Computes the total length. Available for <code class="type">lseg</code>,
          <code class="type">path</code>.
        </div>
        <div>
          <code class="literal">length(path '((-1,0),(1,0))')</code>
          → <code class="returnvalue">4</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.9.1.1.1" class="indexterm"></a>
          <code class="function">npoints</code> (
          <em class="replaceable"><code>geometric_type</code></em> ) →
          <code class="returnvalue">integer</code>
        </div>
        <div>
          Returns the number of points. Available for <code class="type">path</code>,
          <code class="type">polygon</code>.
        </div>
        <div>
          <code class="literal">npoints(path '[(0,0),(1,1),(2,0)]')</code>
          → <code class="returnvalue">3</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.10.1.1.1" class="indexterm"></a>
          <code class="function">pclose</code> ( <code class="type">path</code> ) →
          <code class="returnvalue">path</code>
        </div>
        <div>Converts path to closed form.</div>
        <div>
          <code class="literal">pclose(path '[(0,0),(1,1),(2,0)]')</code>
          → <code class="returnvalue">((0,0),(1,1),(2,0))</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.11.1.1.1" class="indexterm"></a>
          <code class="function">popen</code> ( <code class="type">path</code> ) →
          <code class="returnvalue">path</code>
        </div>
        <div>Converts path to open form.</div>
        <div>
          <code class="literal">popen(path '((0,0),(1,1),(2,0))')</code>
          → <code class="returnvalue">[(0,0),(1,1),(2,0)]</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.12.1.1.1" class="indexterm"></a>
          <code class="function">radius</code> ( <code class="type">circle</code> ) →
          <code class="returnvalue">double precision</code>
        </div>
        <div>Computes radius of circle.</div>
        <div>
          <code class="literal">radius(circle '&lt;(0,0),2&gt;')</code>
          → <code class="returnvalue">2</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.13.1.1.1" class="indexterm"></a>
          <code class="function">slope</code> ( <code class="type">point</code>,
          <code class="type">point</code> ) → <code class="returnvalue">double precision</code>
        </div>
        <div>Computes slope of a line drawn through the two points.</div>
        <div>
          <code class="literal">slope(point '(0,0)', point '(2,1)')</code>
          → <code class="returnvalue">0.5</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.6.2.2.14.1.1.1" class="indexterm"></a>
          <code class="function">width</code> ( <code class="type">box</code> ) →
          <code class="returnvalue">double precision</code>
        </div>
        <div>Computes horizontal size of box.</div>
        <div>
          <code class="literal">width(box '(1,2),(0,0)')</code>
          → <code class="returnvalue">1</code>
        </div>
      </td>
    </tr>
  </tbody>
</table>
</figure>

[#id](#FUNCTIONS-GEOMETRY-CONV-TABLE)

**Table 9.38. Geometric Type Conversion Functions**

<figure class="table-wrapper">
<table class="table" summary="Geometric Type Conversion Functions" border="1">
  <colgroup>
    <col />
  </colgroup>
  <thead>
    <tr>
      <th class="func_table_entry">
        <div class="func_signature">Function</div>
        <div>Description</div>
        <div>Example(s)</div>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.7.2.2.1.1.1.1" class="indexterm"></a>
          <code class="function">box</code> ( <code class="type">circle</code> ) →
          <code class="returnvalue">box</code>
        </div>
        <div>Computes box inscribed within the circle.</div>
        <div>
          <code class="literal">box(circle '&lt;(0,0),2&gt;')</code>
          →
          <code class="returnvalue">(1.414213562373095,1.414213562373095),​(-1.414213562373095,-1.414213562373095)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">box</code> ( <code class="type">point</code> ) →
          <code class="returnvalue">box</code>
        </div>
        <div>Converts point to empty box.</div>
        <div>
          <code class="literal">box(point '(1,0)')</code>
          → <code class="returnvalue">(1,0),(1,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">box</code> ( <code class="type">point</code>,
          <code class="type">point</code> ) → <code class="returnvalue">box</code>
        </div>
        <div>Converts any two corner points to box.</div>
        <div>
          <code class="literal">box(point '(0,1)', point '(1,0)')</code>
          → <code class="returnvalue">(1,1),(0,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">box</code> ( <code class="type">polygon</code> ) →
          <code class="returnvalue">box</code>
        </div>
        <div>Computes bounding box of polygon.</div>
        <div>
          <code class="literal">box(polygon '((0,0),(1,1),(2,0))')</code>
          → <code class="returnvalue">(2,1),(0,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.7.2.2.5.1.1.1" class="indexterm"></a>
          <code class="function">bound_box</code> ( <code class="type">box</code>,
          <code class="type">box</code> ) → <code class="returnvalue">box</code>
        </div>
        <div>Computes bounding box of two boxes.</div>
        <div>
          <code class="literal">bound_box(box '(1,1),(0,0)', box '(4,4),(3,3)')</code>
          → <code class="returnvalue">(4,4),(0,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.7.2.2.6.1.1.1" class="indexterm"></a>
          <code class="function">circle</code> ( <code class="type">box</code> ) →
          <code class="returnvalue">circle</code>
        </div>
        <div>Computes smallest circle enclosing box.</div>
        <div>
          <code class="literal">circle(box '(1,1),(0,0)')</code>
          → <code class="returnvalue">&lt;(0.5,0.5),0.7071067811865476&gt;</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">circle</code> ( <code class="type">point</code>,
          <code class="type">double precision</code> ) → <code class="returnvalue">circle</code>
        </div>
        <div>Constructs circle from center and radius.</div>
        <div>
          <code class="literal">circle(point '(0,0)', 2.0)</code>
          → <code class="returnvalue">&lt;(0,0),2&gt;</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">circle</code> ( <code class="type">polygon</code> ) →
          <code class="returnvalue">circle</code>
        </div>
        <div>
          Converts polygon to circle. The circle's center is the mean of the positions of the
          polygon's points, and the radius is the average distance of the polygon's points from that
          center.
        </div>
        <div>
          <code class="literal">circle(polygon '((0,0),(1,3),(2,0))')</code>
          → <code class="returnvalue">&lt;(1,1),1.6094757082487299&gt;</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.7.2.2.9.1.1.1" class="indexterm"></a>
          <code class="function">line</code> ( <code class="type">point</code>,
          <code class="type">point</code> ) → <code class="returnvalue">line</code>
        </div>
        <div>Converts two points to the line through them.</div>
        <div>
          <code class="literal">line(point '(-1,0)', point '(1,0)')</code>
          → <code class="returnvalue">{0,-1,0}</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.7.2.2.10.1.1.1" class="indexterm"></a>
          <code class="function">lseg</code> ( <code class="type">box</code> ) →
          <code class="returnvalue">lseg</code>
        </div>
        <div>Extracts box's diagonal as a line segment.</div>
        <div>
          <code class="literal">lseg(box '(1,0),(-1,0)')</code>
          → <code class="returnvalue">[(1,0),(-1,0)]</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">lseg</code> ( <code class="type">point</code>,
          <code class="type">point</code> ) → <code class="returnvalue">lseg</code>
        </div>
        <div>Constructs line segment from two endpoints.</div>
        <div>
          <code class="literal">lseg(point '(-1,0)', point '(1,0)')</code>
          → <code class="returnvalue">[(-1,0),(1,0)]</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.7.2.2.12.1.1.1" class="indexterm"></a>
          <code class="function">path</code> ( <code class="type">polygon</code> ) →
          <code class="returnvalue">path</code>
        </div>
        <div>Converts polygon to a closed path with the same list of points.</div>
        <div>
          <code class="literal">path(polygon '((0,0),(1,1),(2,0))')</code>
          → <code class="returnvalue">((0,0),(1,1),(2,0))</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.7.2.2.13.1.1.1" class="indexterm"></a>
          <code class="function">point</code> ( <code class="type">double precision</code>,
          <code class="type">double precision</code> ) → <code class="returnvalue">point</code>
        </div>
        <div>Constructs point from its coordinates.</div>
        <div>
          <code class="literal">point(23.4, -44.5)</code>
          → <code class="returnvalue">(23.4,-44.5)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">point</code> ( <code class="type">box</code> ) →
          <code class="returnvalue">point</code>
        </div>
        <div>Computes center of box.</div>
        <div>
          <code class="literal">point(box '(1,0),(-1,0)')</code>
          → <code class="returnvalue">(0,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">point</code> ( <code class="type">circle</code> ) →
          <code class="returnvalue">point</code>
        </div>
        <div>Computes center of circle.</div>
        <div>
          <code class="literal">point(circle '&lt;(0,0),2&gt;')</code>
          → <code class="returnvalue">(0,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">point</code> ( <code class="type">lseg</code> ) →
          <code class="returnvalue">point</code>
        </div>
        <div>Computes center of line segment.</div>
        <div>
          <code class="literal">point(lseg '[(-1,0),(1,0)]')</code>
          → <code class="returnvalue">(0,0)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">point</code> ( <code class="type">polygon</code> ) →
          <code class="returnvalue">point</code>
        </div>
        <div>Computes center of polygon (the mean of the positions of the polygon's points).</div>
        <div>
          <code class="literal">point(polygon '((0,0),(1,1),(2,0))')</code>
          → <code class="returnvalue">(1,0.3333333333333333)</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <a id="id-1.5.8.17.7.2.2.18.1.1.1" class="indexterm"></a>
          <code class="function">polygon</code> ( <code class="type">box</code> ) →
          <code class="returnvalue">polygon</code>
        </div>
        <div>Converts box to a 4-point polygon.</div>
        <div>
          <code class="literal">polygon(box '(1,1),(0,0)')</code>
          → <code class="returnvalue">((0,0),(0,1),(1,1),(1,0))</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">polygon</code> ( <code class="type">circle</code> ) →
          <code class="returnvalue">polygon</code>
        </div>
        <div>Converts circle to a 12-point polygon.</div>
        <div>
          <code class="literal">polygon(circle '&lt;(0,0),2&gt;')</code>
          →
          <code class="returnvalue">((-2,0),​(-1.7320508075688774,0.9999999999999999),​(-1.0000000000000002,1.7320508075688772),​(-1.2246063538223773e-16,2),​(0.9999999999999996,1.7320508075688774),​(1.732050807568877,1.0000000000000007),​(2,2.4492127076447545e-16),​(1.7320508075688776,-0.9999999999999994),​(1.0000000000000009,-1.7320508075688767),​(3.673819061467132e-16,-2),​(-0.9999999999999987,-1.732050807568878),​(-1.7320508075688767,-1.0000000000000009))</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">polygon</code> ( <code class="type">integer</code>,
          <code class="type">circle</code> ) → <code class="returnvalue">polygon</code>
        </div>
        <div>
          Converts circle to an <em class="replaceable"><code>n</code></em>-point polygon.
        </div>
        <div>
          <code class="literal">polygon(4, circle '&lt;(3,0),1&gt;')</code>
          → <code class="returnvalue">((2,0),​(3,1),​(4,1.2246063538223773e-16),​(3,-1))</code>
        </div>
      </td>
    </tr>
    <tr>
      <td class="func_table_entry">
        <div class="func_signature">
          <code class="function">polygon</code> ( <code class="type">path</code> ) →
          <code class="returnvalue">polygon</code>
        </div>
        <div>Converts closed path to a polygon with the same list of points.</div>
        <div>
          <code class="literal">polygon(path '((0,0),(1,1),(2,0))')</code>
          → <code class="returnvalue">((0,0),(1,1),(2,0))</code>
        </div>
      </td>
    </tr>
  </tbody>
</table>
</figure>

It is possible to access the two component numbers of a `point` as though the point were an array with indexes 0 and 1. For example, if `t.p` is a `point` column then `SELECT p[0] FROM t` retrieves the X coordinate and `UPDATE t SET p[1] = ...` changes the Y coordinate. In the same way, a value of type `box` or `lseg` can be treated as an array of two `point` values.
