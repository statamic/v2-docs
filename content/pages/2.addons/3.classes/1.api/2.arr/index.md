title: Arr
class: 'Statamic\API\Arr'
overview: "Arr is short for Array. Here you'll find a number of ways to manipulate arrays."
methods:
  -
    name: assoc
    description: |
      Checks if an array is associative

      ```
      $is_assoc = Arr::assoc($array);
      ```
    signature: $array
  -
    name: combineRecursive
    description: |
      Deep merges arrays better than `array_merge_recursive()`.

      ```
      $arr1 = [
          'foo' => 'bar',
          'baz' => 'qux'
      ];
      $arr2 = [
          'hello' => 'world',
          'baz' => [
              'bacon' => 'strips'
          ]
      ];

      $arr = Arr::combineRecursive($arr1, $arr2);
      ```

      ``` .language-output
      "foo" => "bar",
      "baz" => [
        "bacon" => "strips"
      ],
      "hello" => "world"
      ```
    signature: $array1, $array2
id: 0ac74324-79ad-4d34-80e5-9292692a18c9
