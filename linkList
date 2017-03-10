# -*- coding: cp936 -*-
'''
这是一个静态单链表的实现
同时实现了一些magic method用于进行iter或者slice等等
需要注意的是，代码中的游标（指针）和key不是同一个概念
cur(pointer)是这个静态链表元素的实际下标
key是这个单链表元素的逻辑下标

未完，以后再写吧，写了一上午了，有些懒
2017.03.10
平善明
'''


class LinkList():
    # a linklist implement by list with static
    # 2017.03.10
    # psm
    def __init__(self, li=[]):
        self.size = 10  # the real size of ll
        self.li = [[None, i + 1] for i in range(self.size)]  # allocate
        self.li[self.size - 1][1] = None  # the tail point None
        self.li[0][0] = self.size - 2  # the length of unused space
        self.li[0][1] = 2  # the pointer to the first unused space
        self.li[1][0] = 0  # the length of ll(used space)
        self.li[1][1] = None  # the pointer to the first used space

        if li != []:  # add the items to the ll
            self(li)
    def insert(self, x):
        # insert x at the begining of the LinkList
        if self.li[0][0] < 1:
            self.expandSize()  # if no space,expand the ll
        nullCur = self.li[0][1]  # the pointer to the first unused space
        self.li[0][0] -= 1  # unused space minus one
        self.li[1][0] += 1  # used space add one
        self.li[0][1] = self.li[nullCur][1]  # move unused pointer
        self.li[nullCur][0] = x  # give the value to the new item
        self.li[nullCur][1] = self.li[1][1]
        self.li[1][1] = nullCur  # head link new node
        return self
    def append(self, x):
        # insert x at the end of the LinkList
        if self.li[0][0] < 1:
            self.expandSize()  # if no space,expand the ll
        tail = 1
        while self.li[tail][1] != None:
            tail = self.li[tail][1]  # get the tail of ll
        nullCur = self.li[0][1]  # the pointer to the first unused space
        self.li[0][0] -= 1  # unused space minus one
        self.li[1][0] += 1  # used space add one
        self.li[0][1] = self.li[nullCur][1]  # move unused pointer
        self.li[nullCur][0] = x  # give the value to the new item
        self.li[nullCur][1] = self.li[tail][1]  # new node become tail
        self.li[tail][1] = nullCur  # old tail link new tail
        return self
    def delete(self, delCur):
        self.li[1][0] -= 1  # used space minus one
        self.li[0][0] += 1  # unused space add one
        self.li[delCur][0] = None  # del the value
        cur = 1
        while self.li[cur][1] != delCur:
            cur = self.li[cur][1]  # get the ahead pointer
        self.li[cur][1] = self.li[delCur][1]  # the ahead pointer link the hinder item
        self.li[delCur][1] = self.li[0][1]  # add the item to unused list
        self.li[0][1] = delCur
        return self
    def expandSize(self):
        self.li[0][1] = self.size
        self.li += [[None, self.size + i + 1] for i in range(self.size - 2)]
        self.li[-1][1] = None
        self.li[0][0] = self.size - 2
        self.size = len(self.li)
    def pHead(self):
        return self.li[1][1]
    def empty(self):
        return self.li[1][0] == 0
    def length(self):
        return self.li[1][0]
    def printUsed(self):
        for i in self:
            print i,
        print
        return self
    def printUnused(self):
        ncur = self.li[0][1]
        while ncur is not None:
            print self.li[ncur],
            ncur = self.li[ncur][1]
        print
        return self

    # cur(or pointer,p) is the index in self.li
    # key is the logical location
    #    [[2, 8], [6, 2], [0, 4], [1, 5], [2, 3], [3, 6], [4, 7], [6, None], [None, 9], [None, None]]
    # cur     0       1      2       3       4       5       6        7        8           9
    # key                    0       2       1       3       4        5
    def getPointerOfKey(self, key):
        # as title,the key is int
        if key < self.length():
            if key >= 0:
                num = 0
                p = self.pHead()
                while num != key:
                    p = self.li[p][1]
                    num += 1
                return p
            elif key < 0 and key > -self.length():
                p0, p1 = self.pHead(), self.pHead()
                for i in range(-key):
                    p1 = ll.li[p1][1]
                while p1 is not None:
                    p0 = ll.li[p0][1]
                    p1 = ll.li[p1][1]
                return p0
            else:
                raise KeyError('no this key\n')
        else:
            raise KeyError('no this key\n')
    def reorganizeSliceKey(self, key):
        # [::]->
        # [::1]->[0:len(self):1]
        # [::-1]->[len(self)-1:-1:-1]
        # [1:]->[1:len(self):1]
        start, stop, step = key.start, key.stop, key.step
        # print start, stop, step
        if key.stop == 9223372036854775807L:
            stop = len(self)
        if start is None and stop is None:
            if step is None:
                print 'haven"t implement'
                return None
            elif step > 0 :
                start = 0
                stop = len(self)
            else:
                start = len(self) - 1
                stop = -1
        if step is None:
            step = 1
        return start, stop, step
    def __len__(self):
        return self.li[1][0]
    def __iter__(self):
        self.ncur = self.li[1][1]  # next item's index
        return self
    def next(self):
        if self.ncur is not None:
            ret = self.li[self.ncur]
            self.ncur = self.li[self.ncur][1]
            return ret
        else:
            raise StopIteration
    def __getitem__(self, key):
        if isinstance(key, int):
            return self.li[self.getPointerOfKey(key)]
        elif isinstance(key, slice):
            start, stop, step = self.reorganizeSliceKey(key)
            # return a new ll
            return LinkList([self.__getitem__(i)[0]
                             for i in range(start, stop, step)])
        else:
            raise TypeError('key must be int or slice\n')
    def __setitem__(self, key, value):
        # one -> one : change the value
        # one -> many : del it, and add new item at this location
        # many -> one : change the first value, del others
        # little -> many : del them, and add new item at this location
        # many -> little : change the ahead values, del others
        if isinstance(key, int):  # one ->
            if not (isinstance(value, list) or isinstance(value, tuple)):  # one -> one
                self.li[self.getPointerOfKey(key)][0] = value
            else:  # one -> many
                pass
        elif isinstance(key, slice):  # many ->
            start, stop, step = key.start, key.stop, key.step
            print start, stop, step
            print 'haven"t implement'
            return None
        else:
            raise TypeError('key must be int or slice\n')
    def __delitem__(self, key):
        # del one
        # del many
        if isinstance(key, int):
            self.delete(self.getPointerOfKey(key))
        elif isinstance(key, slice):
            start, stop, step = key.start, key.stop, key.step
            print start, stop, step
            print 'haven"t implement'
            return None
        else:
            raise TypeError('key must be int or slice\n')
    def __call__(self, li):
        if isinstance(li, list) or isinstance(li, tuple):
            for i in li:  # add all item in li to the LinkList
                self.append(i)
        else:
            self.append(li)  # add item to the LinkList
        return self
    def __reversed__(self):
        pass
    def __contains__(self, item):
        pass

if __name__ == '__main__':
    print '\ninit:'
    ll = LinkList([7, 8])(range(3)).insert(6).append(5).printUsed()

    print '\nall space:'
    print ll.li

    print '\nindex:', ll[-1], ll[0], ll[1]

    print '\nslice:'
    ll[:].printUsed()
    ll[::-1].printUsed()
    ll[1:3].printUsed()
    ll[4:0:-1].printUsed()

    print '\nassignment:'
    ll[3] = 9
    ll[-2] = 'a'
    ll.printUsed()

    print '\ndel:'
    del ll[2]
    del ll[-2]
    ll.printUsed()

    print '\niter:'
    for i in ll:
        print i,
    print

