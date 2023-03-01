# 순열, 조합 by 재귀함수

## 순열(Permutation), 조합(Combination) 구현

```java
import java.util.*;

public class PermutationCombination {

    static int result; // 구하고자 하는 순열, 조합의 개수

    /*
    int[] n : 뽑을 대상 배열
    int r : 뽑을 개수
    int[] out : 뽑은 배열
    int outIndex : 뽑은 배열의 인덱스
    boolean[] visited : 중복 체크를 위한 boolean 배열
    */
    public static void permutation(int[] n, int r, int[] out, int outIndex, boolean[] visited) {
        if(outIndex == r) {
            result++;
            System.out.println(Arrays.toString(out));
            return;
        }

        if(visited == null) { // 중복 순열
            for (int i = 0; i < n.length; i++) {
                    out[outIndex] = n[i];
                    permutation(n, r, out, outIndex + 1, null);
            }
        }else { // 순열
            for (int i = 0; i < n.length; i++) {
                if (!visited[i]) {
                    visited[i] = true;
                    out[outIndex] = n[i];
                    permutation(n, r, out, outIndex + 1, visited);
                    visited[i] = false;
                }
            }
        }
    }

    // 조합
    /*
    int[] n : 뽑을 대상 배열
    int r : 뽑을 개수
    int start : 뽑을 갯수를 만들기 위한 start index
    int end : 뽑을 갯수를 만들기 위한 end index
    boolean[] visited : 중복 체크를 위한 boolean 배열
    */
    public static void combination(int[] n, int r, int start, int end, boolean[] visited) {
        if(end == r) {
            result++;
            return;
        }

        for (int i = start; i < n.length; i++) {
            if(!visited[i]){
                visited[i] = true;
                combination(n, r, i+1, end+1, visited);
                visited[i] = false;
            }
        }
    }

    public static void main(String[] args) {
        int[] n = {1,2,3,4,5};
        int r = 3;
        //permutation(n, r, new int[r], 0, new boolean[n.length]); // 순열
        //permutation(n, r, new int[r], 0, null); // 중복 순열

        /*
        int[] rn : 중복 조합은 공식으로 생성 nCr -> n+r-1Cr
        */
        int[] rn = new int[n.length + r - 1];
        Arrays.setAll(rn, i -> i + 1);

        //combination(n, r, 0, 0, new boolean[n.length]); // 조합
        //combination(rn, r, 0, 0, new boolean[rn.length]); // 중복 조합
        System.out.println(result);
    }
}
```