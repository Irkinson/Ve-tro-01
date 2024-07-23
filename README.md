class _StringDataGeneration(object):

    def __init__(self):
        self._overall_pattern = re.compile(
            r'((?:[dcw]{1}(?!\*\d+))|(?:[dcw]{1}(?:\*\d+))|[\D\W\S])')
        self._expression_pattern = re.compile(r'[dcw]{1}(?:\*\d+)')

        self.SUPPORTED_CHARS = {
            'c': self._generate_chars,
            'd': self._generate_numbers,
            'w': self._generate_chars_and_numbers,
        }

    def get_random_string_by_pattern(self, format):
        """Generate random string by pattern:
        Examples:
        | ${string} | Get Random String By Pattern | ddd*10d-c*2ccc-d*2-cc*2-w-w*5ww | """
        groups = self._overall_pattern.findall(format)
        return "".join(map(self._replace, groups))

    def _replace(self, pattern):
        if self._expression_pattern.search(pattern):
            parsed_expression = pattern.split('*')
            return self.SUPPORTED_CHARS[parsed_expression[0]](int(parsed_expression[1]))
        elif pattern in self.SUPPORTED_CHARS.keys():
            return self.SUPPORTED_CHARS[pattern]()
        else:
            return pattern
..
