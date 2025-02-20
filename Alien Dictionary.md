## Alien Dictionary
A new alien language uses the English alphabet, but the order of letters is unknown. You are given a list of words[] from the alien language’s dictionary, where the words are claimed to be sorted lexicographically according to the language’s rules.

Your task is to determine the correct order of letters in this alien language based on the given words. If the order is valid, return a string containing the unique letters in lexicographically increasing order as per the new language's rules. If there are multiple valid orders, return any one of them.

However, if the given arrangement of words is inconsistent with any possible letter ordering, return an empty string ("").

A string a is lexicographically smaller than a string b if, at the first position where they differ, the character in a appears earlier in the alien language than the corresponding character in b. If all characters in the shorter word match the beginning of the longer word, the shorter word is considered smaller.

Your implementation will be tested using a driver code. It will print true if your returned order correctly follows the alien language’s lexicographic rules; otherwise, it will print false.

Examples:

Input: words[] = ["cb", "cba", "a", "bc"]  
Output: true  
Explanation: You need to return "cab" as the correct order of letters in the alien dictionary.  
Input: words[] = ["ab", "aa", "a"]  
Output: ""  
Explanation: You need to return "" because "aa" is lexicographically larger than "a", making the order invalid.  
Input: words[] = ["ab", "cd", "ef", "ad"]  
Output: ""  
Explanation: You need to return "" because "a" appears before "e", but then "e" appears before "a", which contradicts the ordering rules.  
Constraints:  

1 <= words.length <= 500  
1 <= words[i].length <= 100  
words[i] consists only of lowercase English letters  


## Approach
Since we have to find the increasing order of letter according to given words
1.  find all the unique character in given array by traversing each word of the array
2.  compare two string - iterate while character is same in both string.
     i.  if distinct character is found then then edge is found from char in first string to char in second string.
     ii.  if there is no distinct character is found - then if length of first string is greater than length of second string, then we cannot find the order, hence return empty string.
3.  apply the topological sorting , starting from  making indegree
4.  initialize queue to add node with o indegree
5.  add node only if it is present in the unique character array which was checked in first step
6.  proceed with this.
7.  after doing so, if the number of traversed node is not equal to the number of unique character then order is invalid hence return empty string, otherwise return the formed string.

 ```
    
class Solution {
    public String findOrder(String[] words) {
        // code here
        List<List<Integer>> adj=new ArrayList<>();
        for(int i=0; i<26; i++){
            adj.add(new ArrayList<>());
        }
        boolean vis[]=new boolean[26];
        for(String s : words){
            for(char ch : s.toCharArray()){
                vis[ch-'a']=true;
            }
        }
        
        
        for(int i=0; i<words.length-1; i++){
            String s1=words[i];
            String s2=words[i+1];
            boolean flag=false;
            int min =Math.min(s1.length(),s2.length());
            for(int j=0; j<min; j++){
                if(s1.charAt(j)!=s2.charAt(j)){
                    adj.get(s1.charAt(j)-'a').add(s2.charAt(j)-'a');
                    flag=true;
                    break;
                }
            }
            if(!flag && s1.length()>s2.length()){
                return "";
            }
        }
        // applying topo sort
        int indegree[]=new int[26];
        for(int i=0; i<26; i++){
            for(int node : adj.get(i)){
                indegree[node]++;
            }
        }
        Queue<Integer> q= new LinkedList<>();
        for(int i=0; i<26; i++){
            if(vis[i] && indegree[i]==0){
                q.add(i);
            }
        }
        StringBuilder str = new StringBuilder();
        while(!q.isEmpty()){
            int node = q.remove();
            str.append((char)(node+'a'));
            for(int adjNode : adj.get(node)){
                indegree[adjNode]--;
                if(indegree[adjNode]==0){
                    q.add(adjNode);
                }
            }
        }
        // count number of unique character 
         int count=0;
         for(boolean flag : vis){
             if(flag){
                 count++;
             }
         }
        return str.length()==count ? str.toString() : "";
    }
}
