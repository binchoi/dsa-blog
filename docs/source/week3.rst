************************
Week 3 [25-31 Oct 2021]
************************
Day 13 [25 Oct]
================
Question 31: XXX
--------------------------------

My first failed attempt:: 

    def __init__(self): 
        self.res = False       

    def wordBreakHelper(self, remainingStr: str, wordDict: List[str]):
        if remainingStr.replace("|", "") == "": 
            self.res = True
            return
        else: 
            for word in wordDict: 
                if self.res: 
                    return
                foundIdx = remainingStr.find(word)
                if foundIdx == -1: 
                    continue
                self.wordBreakHelper((remainingStr[:foundIdx] + "|"+ remainingStr[foundIdx+ len(word):]), wordDict)
                        
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:        
        self.wordBreakHelper(s, wordDict)
        return self.res
        
