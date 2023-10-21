#! /usr/bin/env python
import os
import argparse
import re

# 函数，自然数转汉语


def num2chinese(d):
    if d == 0:
        return '零'
    if 100000000 > d > 0:
        num = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九']
        kin = ['十', '百', '千', '万']
        num_str = list(str(d))
        num_str.reverse()
        chinese_str = ""
        for index, i in enumerate(num_str):
            if index != 0:
                if i != "0":
                    chinese_str = num[int(i)] + \
                        kin[(index % 4)-1] + chinese_str
                elif chinese_str:
                    if index == 4:
                        if chinese_str[0] != "零":
                            chinese_str = kin[3] + "零" + chinese_str
                        else:
                            chinese_str = kin[3] + chinese_str
                    if not(chinese_str[0] in ["零", "万"] and i == "0"):
                        chinese_str = num[int(i)] + chinese_str

            else:
                if i != "0":
                    chinese_str = num[int(i)] + chinese_str
        if chinese_str[:2] == "一十":
            chinese_str = chinese_str[1:]
        return chinese_str

# 函数 自然数转英语


def num2english(num):
    d = {0: 'zero', 1: 'one', 2: 'two', 3: 'three', 4: 'four', 5: 'five',
         6: 'six', 7: 'seven', 8: 'eight', 9: 'nine', 10: 'ten',
         11: 'eleven', 12: 'twelve', 13: 'thirteen', 14: 'fourteen',
         15: 'fifteen', 16: 'sixteen', 17: 'seventeen', 18: 'eighteen',
         19: 'nineteen', 20: 'twenty',
         30: 'thirty', 40: 'forty', 50: 'fifty', 60: 'sixty',
         70: 'seventy', 80: 'eighty', 90: 'ninety'}
    k = 1000
    m = k * 1000
    b = m * 1000
    t = b * 1000

    assert(0 <= num)

    if (num < 20):
        return d[num]

    if (num < 100):
        if num % 10 == 0:
            return d[num]
        else:
            return d[num // 10 * 10] + '-' + d[num % 10]

    if (num < k):
        if num % 100 == 0:
            return d[num // 100] + ' hundred'
        else:
            return d[num // 100] + ' hundred and ' + num2english(num % 100)

    if (num < m):
        if num % k == 0:
            return num2english(num // k) + ' thousand'
        else:
            return num2english(num // k) + ' thousand, ' + num2english(num % k)

    if (num < b):
        if (num % m) == 0:
            return num2english(num // m) + ' million'
        else:
            return num2english(num // m) + ' million, ' + num2english(num % m)

    if (num < t):
        if (num % b) == 0:
            return num2english(num // b) + ' billion'
        else:
            return num2english(num // b) + ' billion, ' + num2english(num % b)

    if (num % t == 0):
        return num2english(num // t) + ' trillion'
    else:
        return num2english(num // t) + ' trillion, ' + num2english(num % t)

# 数字转罗马


def num2roman(num: int) -> str:
    a = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
    b = ['M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I']
    res = ''
    for i, n in enumerate(a):
        while num >= a[i]:
            res += b[i]
            num -= a[i]
    return res


parser = argparse.ArgumentParser(
    description='批量重命名当前路径的文件', prefix_chars='-+')
# 输入方式，默认为linuxshell通配符，此项为true则更改为正则表达式
parser.add_argument('-r', '--regexp', help='输入方式，默认为文件名（或者部分文件名或者linuxshell通配符），此项为true则更改为正则表达式',
                    action='store_true')
# 输入文件名，此参数可以有无限个，如果没有则默认为当前路径下所有文件
parser.add_argument('filenames', help='输入文件名（或通配符），此参数可以有无限个，如果没有则默认为当前路径下所有文件',
                    type=str, nargs='*', default=None, metavar='FileNames')
# 指定排序开始数字，默认为0
parser.add_argument('-c', '--count', help='指定排序开始数字，默认为0',
                    type=int, default=0, metavar='Count')
# 指定排序步长，默认为1
parser.add_argument('-T', '--step', help='指定排序步长，默认为1',
                    type=int, default=1, metavar='Step')
# 指定排序数字类型，默认为阿拉伯数字，可选汉字、英文、罗马数字
parser.add_argument('-t', '--type', help='指定排序数字类型，默认为阿拉伯数字，可选汉字(chinese)、英文(english)、罗马数字(roman)、阿拉伯数字补齐零(fillzero))',
                    type=str, default='num', metavar='Type', choices=['num', 'chinese', 'english', 'roman', 'fillzero'])
# 指定排序类型，默认为不排序，可选文件名排序，可选文件创建时间排序，文件修改时间排序，文件大小排序，文件拓展名排序
parser.add_argument('-m', '--method', help='指定排序类型，默认为不排序，可选文件名排序(name)，可选文件创建时间排序(ctime)，文件修改时间排序(mtime)，文件大小排序(size)，文件拓展名排序(extension)',
                    type=str, default=None, metavar='Method', choices=['name', 'ctime', 'mtime', 'size', 'extension'])
# 指定排序顺序，默认为升序，可选降序
parser.add_argument('-o', '--order', help='指定排序顺序，默认为升序(asc)，可选降序(desc)',
                    type=str, default='asc', metavar='Order', choices=['asc', 'desc'])
parser.add_argument('-n', '--newname', help=f'新文件名，可用百分d表示排序数字，可用百分s表示原文件名',
                    type=str, default=None, metavar='NewName')
parser.add_argument('-e', '--extension', help='修改拓展名',
                    type=str, default='', metavar='Extension')
parser.add_argument('-s', '--suffixdelete',
                    help='删除后缀，不包括拓展名', type=str, default='', metavar='SuffixDelete')
parser.add_argument('-p', '--prefixdelete', help='删除前缀',
                    type=str, default='', metavar='PrefixDelete')
parser.add_argument('+s', '--suffixadd',
                    help='添加后缀，不包括拓展名', type=str, default='', metavar='SuffixAdd')
parser.add_argument('+p', '--prefixadd', help='添加前缀',
                    type=str, default='', metavar='PrefixAdd')
# 删除部分字符串
parser.add_argument('-d', '--delete', help='删除部分字符串，不包括拓展名',
                    type=str, default='', metavar='Delete')
# 替换部分字符串，必须有两个参数
parser.add_argument('-a', '--add', help='替换部分字符串，不包括拓展名，参数1要替换的字符串，参数2替换后的字符串',
                    type=str, metavar='Add', nargs=2)
# 正则表达式替换字符串，必须有两个参数
parser.add_argument('-R', '--regexpreplace', help='正则表达式替换字符串，不包括拓展名，参数1正则表达式，参数2替换后的字符串',
                    type=str, metavar='RegexpReplace', nargs=2)
# 替换数字，必须有两个参数
parser.add_argument('-N', '--numreplace', help='替换数字，参数1正则表达式（选择好你要替换的数字），参数2替换后的数字类型，可选汉字(chinese)、英文(english)、罗马数字(roman)，若有多个匹配，本操作只替换第一个',
                    type=str, metavar='NumReplace', nargs=2)
# 切片操作，切取字符串的一部分，最多有两个参数
parser.add_argument('-S', '--slice', help='切片操作，切取文str件名的一部分（不包括拓展名）',
                    type=int, metavar='Slice', nargs='*')

args = parser.parse_args()
flag = 0
# 判断是否启用正则表达式
if args.regexp:
    # 正则表达式匹配文件名，只需要部分匹配即可
    filelist = [f for f in os.listdir('.') if re.search(args.filenames[0], f)]
else:
    # 检查filename是否有多个参数
    if len(args.filenames) > 1:
        # 如果有多个参数，则将filelist设置为filename
        filelist = args.filenames
    elif len(args.filenames) == 1:
        # 如果为一个参数，则将所有包含filename的文件名添加到filelist
        filelist = [f for f in os.listdir('.') if args.filenames[0] in f]
    else:
        # 如果没有参数，则将所有文件名添加到filelist
        filelist = os.listdir('.')

# 对filelist进行排序，排序方法由method参数决定，排序顺序由order参数决定
if args.method == 'name':
    filelist.sort(key=lambda x: x, reverse=True if args.order ==
                  'desc' else False)
elif args.method == 'ctime':
    filelist.sort(key=lambda x: os.path.getctime(
        x), reverse=True if args.order == 'desc' else False)
elif args.method == 'mtime':
    filelist.sort(key=lambda x: os.path.getmtime(
        x), reverse=True if args.order == 'desc' else False)
elif args.method == 'size':
    filelist.sort(key=lambda x: os.path.getsize(
        x), reverse=True if args.order == 'desc' else False)
elif args.method == 'extension':
    filelist.sort(key=lambda x: os.path.splitext(
        x)[1], reverse=True if args.order == 'desc' else False)

count = args.count
for f in filelist:
    # 根据排序数字类型，将数字转换为对应的字符串
    if args.type == 'num':
        countstr = str(count)
    elif args.type == 'chinese':
        countstr = num2chinese(count)
    elif args.type == 'english':
        countstr = num2english(count)
    elif args.type == 'roman':
        countstr = num2roman(count)
    elif args.type == 'fillzero':
        # 计算最大数字，最大数字开始时args.count，步长时args.step，最大数字为args.count+args.step*len(filelist)
        maxnum = args.count+args.step*len(filelist)
        # 按照最大数字的位数，将数字转换为对应的字符串
        countstr = str(count).zfill(len(str(maxnum)))
    # 获取文件名和拓展名
    fname, file_extension = os.path.splitext(f)

    # 如果有新文件名操作，则执行新文件名操作
    if args.newname:
        tempname = args.newname
        # 如果新文件名中有%d，则替换为数字
        if f"%d" in args.newname:
            tempname = tempname.replace(f"%d", countstr)
        # 如果新文件名中有%s，则替换为原文件名
        if f"%s" in args.newname:
            tempname = tempname.replace(f"%s", fname)
        # 如果没有%d和%s，且文件数量不止一个，则在新文件名后添加数字
        if f"%d" not in args.newname and f"%s" not in args.newname and len(filelist) > 1:
            tempname = tempname+countstr
        fname = tempname
    # 修改拓展名
    if args.extension:
        # 如果用户没有输入拓展名前的点，则自动添加
        if args.extension[0] != '.':
            args.extension = '.'+args.extension
        file_extension = args.extension
    # 删除后缀
    if args.suffixdelete:
        # 检查是否有此后缀，如果没有则跳过
        if fname[-len(args.suffixdelete):] == args.suffixdelete:
            fname = fname[:-len(args.suffixdelete)]
    # 删除前缀
    if args.prefixdelete:
        # 检查是否有此前缀，如果没有则跳过
        if fname[:len(args.prefixdelete)] == args.prefixdelete:
            fname = fname[len(args.prefixdelete):]
    # 添加后缀
    if args.suffixadd:
        tempsuffix = args.suffixadd
        # 如果后缀中有%d，则替换为数字
        if f"%d" in args.suffixadd:
            tempsuffix = tempsuffix.replace(f"%d", countstr)
        # 如果后缀中有%s，则替换为原文件名
        if f"%s" in args.suffixadd:
            tempsuffix = tempsuffix.replace(f"%s", fname)
        fname = fname+tempsuffix
    # 添加前缀
    if args.prefixadd:
        tempprefix = args.prefixadd
        # 如果前缀中有%d，则替换为数字
        if f"%d" in args.prefixadd:
            tempprefix = tempprefix.replace(f"%d", countstr)
        # 如果前缀中有%s，则替换为原文件名
        if f"%s" in args.prefixadd:
            tempprefix = tempprefix.replace(f"%s", fname)
        fname = args.prefixadd+fname
    # 删除部分字符串
    if args.delete:
        # 如果不存在此字符串，跳过
        if args.delete not in fname:
            continue
        fname = fname.replace(args.delete, '')
    # 替换部分字符串
    if args.add:
        tempadd = args.add
        # 如果不存在此字符串，跳过
        if args.add[0] not in fname:
            continue
        # 如果替换字符中有%d，则替换为数字
        if f"%d" in args.add[1]:
            tempadd = tempadd.replace(f"%d", countstr)
        # 如果替换字符中有%s，则替换为原文件名
        if f"%s" in args.add[1]:
            tempadd = tempadd.replace(f"%s", fname)
        fname = fname.replace(args.add[0], args.add[1])
    # 正则表达式替换字符串
    if args.regexpreplace:
        # 如果没有匹配到，跳过
        if not re.search(args.regexpreplace[0], fname):
            continue
        fname = re.sub(
            args.regexpreplace[0], args.regexpreplace[1], fname)
    # 替换数字
    if args.numreplace:
        # 根据第一个参数用正则表达式匹配到文件名中的数字字符串，如果没有匹配到，则跳过
        if not re.search(args.numreplace[0], fname):
            continue
        numstr = re.search(args.numreplace[0], fname).group()
        # 验证数字字符串是否可以转换为自然数，否则报错
        try:
            num = int(numstr)
        except ValueError:
            print('错误：正则表达式匹配到的数字字符串无法转换为自然数')
            exit()
        if num < 0:
            print('错误：正则表达式匹配到的数字字符串无法转换为自然数')
            exit()
        # 根据第二个参数，将数字转换为对应的字符串
        if args.numreplace[1] == 'chinese':
            rs = num2chinese(num)
        elif args.numreplace[1] == 'english':
            rs = num2english(num)
        elif args.numreplace[1] == 'roman':
            rs = num2roman(num)
        # 替换数字字符串
        fname = fname.replace(numstr, rs)
    # 切片操作
    if args.slice:
        # tempslice 是 args.slice的浅拷贝
        tempslice = args.slice.copy()
        # 如果没有参数，报错
        if len(tempslice) == 0:
            print('错误：切片操作必须至少有一个参数')
            exit()
        # 如果超过一个参数，则输出警告
        if len(tempslice) > 2:
            print('警告：切片操作只能有两个参数，多余的参数将被忽略')
        # 如果切片操作的起始位置大于文件名长度，跳过
        if tempslice[0] > len(fname):
            continue
        # 如果只有一个参数，则将结束位置设置为文件名长度
        if len(tempslice) == 1:
            tempslice.append(len(fname))
        # 如果切片操作的结束位置大于文件名长度，将结束位置设置为文件名长度
        if tempslice[1] > len(fname):
            tempslice[1] = len(fname)

        # 切片操作
        fname = fname[tempslice[0]:tempslice[1]]

    # 重命名文件
    os.rename(f, fname+file_extension)
    count += args.step
