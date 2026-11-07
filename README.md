class Solution {
public:
    int sumCounts(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;

        for (int i = 0; i < n; i++) {
            unordered_set<int> st;

            for (int j = i; j < n; j++) {
                st.insert(nums[j]);
                int distinct = st.size();
                ans += distinct * distinct;
            }
        }

        return ans;
    }
};# SEP-29
class Solution {
public:
    int countDaysTogether(string arriveAlice, string leaveAlice,
                          string arriveBob, string leaveBob) {

        int days[] = {31, 28, 31, 30, 31, 30,
                      31, 31, 30, 31, 30, 31};

        auto convert = [&](string date) {
            int month = stoi(date.substr(0, 2));
            int day = stoi(date.substr(3, 2));

            for (int i = 0; i < month - 1; i++) {
                day += days[i];
            }
            return day;
        };

        
class Solution {
public:
    int m, n;

    void dfs(vector<vector<int>>& grid, int i, int j) {
        if (i < 0 || j < 0 || i >= m || j >= n)
            return;

        if (grid[i][j] == 1)
            return;

        grid[i][j] = 1;

        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }

    int closedIsland(vector<vector<int>>& grid) {
        m = grid.size();
        n = grid[0].size();

        for (int i = 0; i < m; i++) {
            dfs(grid, i, 0);
            dfs(grid, i, n - 1);
        }

        for (int j = 0; j < n; j++) {
            dfs(grid, 0, j);
            dfs(grid, m - 1, j);
        }

        int count = 0;

        for (int i = 1; i < m - 1; i++) {
            for (int j = 1; j < n - 1; j++) {
                if (grid[i][j] == 0) {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }

        return count;
    }
};


        int start = max(convert(arriveAlice), convert(arriveBob));
        int end = min(convert(leaveAlice), convert(leaveBob));

        return max(0, end - start + 1);
    }
};
**
class Solution {
public:
    int m, n;
    
    long long dfs(vector<vector<int>>& grid, int i, int j) {
        if (i < 0 || j < 0 || i >= m || j >= n || grid[i][j] == 0)
            return 0;
        
        long long sum = grid[i][j];
        grid[i][j] = 0; // Mark visited
        
        sum += dfs(grid, i + 1, j);
        sum += dfs(grid, i - 1, j);
        sum += dfs(grid, i, j + 1);
        sum += dfs(grid, i, j - 1);
        
        return sum;
    }
    
    int countIslands(vector<vector<int>>& grid, int k) {
        m = grid.size();
        n = grid[0].size();
        int count = 0;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] > 0) {
                    long long sum = dfs(grid, i, j);
                    if (sum % k == 0)
                        count++;
                }
            }
            
class Solution {
public:
    int uniquePaths(vector<vector<int>>& grid) {
        const int MOD = 1e9 + 7;

        int m = grid.size();
        int n = grid[0].size();

        vector<vector<vector<int>>> dp(
            m + 1, vector<vector<int>>(n + 1, vector<int>(2, 0))
        );

        // Initial position
        dp[0][0][0] = 1;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {

                int right = dp[i][j][0];
                int down = dp[i][j][1];

                if (grid[i][j] == 0) {
                    int total = (right + down) % MOD;

                    // Move right
                    dp[i][j + 1][0] =
                        (dp[i][j + 1][0] + total) % MOD;

                    // Move down
                    dp[i + 1][j][1] =
                        (dp[i + 1][j][1] + total) % MOD;
                }
                else {
                    // Mirror: right becomes down
                    dp[i + 1][j][1] =
                        (dp[i + 1][j][1] + right) % MOD;

                    // Mirror: down becomes right
                    dp[i][j + 1][0] =
                        (dp[i][j + 1][0] + down) % MOD;
                }
            }
        }

        return (dp[m - 1][n - 1][0] +
                dp[m - 1][n - 1][1]) % MOD;
    }
};

class Solution {
public:
    int minMoves2(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        int median = nums[nums.size() / 2];
        int moves = 0;

        for (int x : nums) {
            moves += abs(x - median);
        }

        return moves;
    }
};
```cpp
class Solution {
public:
    vector<int> assignTasks(vector<int>& servers, vector<int>& tasks) {
        priority_queue<pair<int,int>,
            vector<pair<int,int>>,
            greater<pair<int,int>>> available;

        priority_queue<tuple<long long,int,int>,
            vector<tuple<long long,int,int>>,
            greater<tuple<long long,int,int>>> busy;

        int n = servers.size();
        int m = tasks.size();

        for (int i = 0; i < n; i++) {
            available.push({servers[i], i});
        }

        vector<int> ans(m);
        long long time = 0;

        for (int i = 0; i < m; i++) {
            time = max(time, (long long)i);

            // Release all free servers
            while (!busy.empty() && get<0>(busy.top()) <= time) {
                auto [t, w, idx] = busy.top();
                busy.pop();
                available.push({w, idx});
            }

            // Wait if no server is available
            if (available.empty()) {
                time = get<0>(busy.top());

                while (!busy.empty() && get<0>(busy.top()) <= time) {
                    auto [t, w, idx] = busy.top();
                    busy.pop();
                    available.push({w, idx});
                }
            }

            // Assign task to the best server
            auto [weight, idx] = available.top();
            available.pop();

            ans[i] = idx;

            busy.push({time + tasks[i], weight, idx});
        }

        return ans;
    }
};
```
        }
        
        return count;
    }
};
**
